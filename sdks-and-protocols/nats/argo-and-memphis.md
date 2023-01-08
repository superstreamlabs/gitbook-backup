---
description: Argo and Memphis using NATS Event source
cover: ../../.gitbook/assets/Argo + Memphis.jpeg
coverY: 0
---

# Argo and Memphis

## Introduction

[Argo CD](https://argo-cd.readthedocs.io/en/stable/) is a declarative, GitOps continuous delivery tool for Kubernetes.

Memphis can trigger Argo workflows via Argo EventSource.

An EventSource defines the configurations required to consume events from external sources like Memphis, NATS, AWS SNS, SQS, GCP PubSub, Webhooks, etc.

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
      # subject name
      subject: <a data-footnote-ref href="#user-content-fn-2">foo</a>
      auth:
        token:
          name: <a data-footnote-ref href="#user-content-fn-3">memphis-creds</a>
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
nats stream add  -s <BROKER_URL>:6666 --user=<APPLICATION_USER>::<CONNECTION_TOKEN> 
? Stream Name argo_event_source
? Subjects foo
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
Stream argo_event_source was created
```

[^1]: 

[^2]: \=Memphis station

[^3]: A secret that will be created in step 2
