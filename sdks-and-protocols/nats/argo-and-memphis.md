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

### Step 1: Create event source YAML file.

{% code title="memphis_eventsource.yaml" lineNumbers="true" %}
```
apiVersion: argoproj.io/v1alpha1
kind: EventSource
metadata:
  name: memphis
spec:
  nats:
    example:
      # url of the nats service
      url: nats://<BROKER_URL>:6666
      # jsonBody specifies that all event body payload coming from this
      # source will be JSON
      jsonBody: true
      # subject name
      subject: foo
      auth:
        token:
          name: memphis-creds
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
```
{% endcode %}
