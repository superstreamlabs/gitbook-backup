---
description: Use Datadog as an external monitoring tool to monitor Memphis
cover: ../../../.gitbook/assets/Datadog and Memphis.jpeg
coverY: 0
---

# Datadog

## Introduction

As Datadog is one of the popular tools for centralized monitoring, Memphis provides a Prometheus exporter to enable Datadog users to monitor Memphis in K8S deployment only.

{% hint style="warning" %}
Please make sure you have the [Datadog K8S agent](https://docs.datadoghq.com/containers/kubernetes/installation/?tab=operator) installed.
{% endhint %}

## Getting started

### Step 1: Make sure your Memphis Prometheus exporter is on

If you haven't installed Memphis with the `exporter.enabled` yet - \
(\* `websocket.tls` are optional for a superior GUI experience)

```
helm install memphis memphis \
--create-namespace --namespace memphis --wait \
--set \
cluster.enabled="true",\
exporter.enabled="true", \
websocket.tls.secret.name="tls-secret",\
websocket.tls.cert="memphis_local.pem",\
websocket.tls.key="memphis-key_local.pem",\
```

If Memphis is already installed using Helm -

1. Get the currently installed values of the helm deployment

```
helm get values memphis --namespace memphis

USER-SUPPLIED VALUES:
cluster:
  enabled: true
```

2\.   Run the following command with the already existing parameters from the command above

```
helm upgrade --set cluster.enabled=true --set exporter.enabled=true memphis --namespace memphis
```

### Step 2: Make sure your Memphis Prometheus exporter is on
