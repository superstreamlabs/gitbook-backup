---
description: Argo and Memphis using NATS Event source
cover: ../../.gitbook/assets/Argo + Memphis.jpeg
coverY: 0
---

# Argo

## Introduction

[Argo](https://argoproj.github.io/) is a collection of open-source tools for Kubernetes to run workflows,manage clusters, and do GitOps easily.&#x20;

[Memphis](https://memphis.dev) is an open-source next-generation alternative to traditional message brokers.

Among Argo tools, Argo Workflows can be found.\
Kubernetes-native workflow engine supporting DAG and step-based workflows.\
**Some of Argo Workflows features -**&#x20;

* Define workflows where each step in the workflow is a container.
* Model multi-step workflows as a sequence of tasks or capture the dependencies between tasks using a graph (DAG).
* Easily run compute-intensive jobs for machine learning or data processing in a fraction of the time using Argo Workflows on Kubernetes.
* Run CI/CD pipelines natively on Kubernetes without configuring complex software development products.

Argo-defined workflows can be triggered by incoming events which arrive from a component called "Event Source." \
Multiple event sources can be used simultaneously.

## Using Memphis as an Argo workflows event source

Memphis provides multiple features that enhance the experience with Argo, like:

* Full GUI with real-time observability
* Schema management and transformation
* REST Webhooks
* Dead-Letter Queue with automatic message retransmit
* Serverless stream enrichments
* SDKs: Node.JS, Go, Python, TypeScript, NestJS

One of the key advantages of using Memphis is the ability to troubleshoot complicated async processes using message tracing, data-level observability, self-healing policies, and real-time notifications, which can be a great help, especially for auditing and logging when events and triggers are fired from multiple places towards your Argo and potentially can create highly expensive resources and workflows.

Memphis can trigger Argo workflows via Argo Event Source.

An Event Source defines the configurations required to consume events from external sources like Memphis, NATS, AWS SNS, SQS, GCP PubSub, Webhooks, etc.

<figure><img src="../../.gitbook/assets/argo and memphis.jpeg" alt=""><figcaption><p>Integrating Memphis as a NATS Event Source of Argo</p></figcaption></figure>

## Getting started

### Step 1: Create event source YAML file

<pre class="language-yaml" data-title="memphis_eventsource.yaml" data-line-numbers><code class="lang-yaml">apiVersion: argoproj.io/v1alpha1
kind: EventSource
metadata:
  name: memphis
spec:
  nats:
    example:
      # url of the nats service
      url: nats://&#x3C;<a data-footnote-ref href="#user-content-fn-1">MEMPHIS_BROKER_URL</a>>:6666
      # jsonBody specifies that all event body payload coming from this
      # source will be JSON
      jsonBody: true
      # subject name = memphis station
      subject: argo_workflows
      auth:
        token:
          name: <a data-footnote-ref href="#user-content-fn-2">memphis-creds</a>
          key: CONNECTION_TOKEN
      # optional backoff time for connection retries.
      # if not provided, default connection backoff time will be used.
      connectionBackoff:
        # duration in nanoseconds, or strings like "4s", "1m". following value is 10 seconds
        duration: 10s
        # how many backoffs
        steps: 5
        # factor to increase on each step.
        # setting factor > 1 makes backoff exponential.
        factor: 2
        jitter: 0.2
</code></pre>

### Step 2:  Create `memphis-creds` secret in `argo-evens` namespace

```yaml
kubectl create secret generic memphis-creds \
--from-literal=CONNECTION_TOKEN=<APPLICATION_USER>::<CONNECTION_TOKEN> \
-n argo-events
```

### Step 3: Create Event Source resource

```
kubectl apply -f memphis_eventsource.yaml
```

### Step 4: Create sensor resource YAML file

{% code title="memphis_sensor.yaml" lineNumbers="true" %}
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Sensor
metadata:
  name: memphis
spec:
  template:
    serviceAccountName: operate-workflow-sa
  dependencies:
    - name: test-dep
      eventSourceName: memphis
      eventName: example
  triggers:
    - template:
        name: memphis-workflow-trigger
        k8s:
          operation: create
          source:
            resource:
              apiVersion: argoproj.io/v1alpha1
              kind: Workflow
              metadata:
                generateName: memphis-workflow-
              spec:
                entrypoint: whalesay
                arguments:
                  parameters:
                  - name: message
                    value: hello world
                templates:
                - name: whalesay
                  inputs:
                    parameters:
                    - name: message
                  container:
                    image: docker/whalesay:latest
                    command: [cowsay]
                    args: ["{{inputs.parameters.message}}"]
          parameters:
            - src:
                dependencyName: test-dep
              dest: spec.arguments.parameters.0.value
```
{% endcode %}

### Step 5: Create sensor resource

```
kubectl apply -f memphis_sensor.yaml
```

### Step 6: Wrap the "foo" subject with a stream

As Memphis works with streams, wrapping the subject will enable Memphis to control the subject. To complete that step, [NATS cli](https://github.com/nats-io/natscli) is needed.

```markup
nats stream add  -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_APPLICATION_USER>::<MEMPHIS_CONNECTION_TOKEN> 
? Stream Name argo_workflows
? Subjects argo_workflows
? Storage file
? Replication 3
? Retention Policy Limits
? Discard Policy Old
? Stream Messages Limit -1
? Per Subject Messages Limit -1
? Total Stream Size -1
? Message TTL -1
? Max Message Size -1
? Duplicate tracking time window 2m0s
? Allow message Roll-ups No
? Allow message deletion Yes
? Allow purging subjects or the entire stream Yes
Stream argo_workflows was created
```

[^1]: 

[^2]: A secret that will be created in step 2
