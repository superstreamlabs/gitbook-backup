---
description: Use Datadog as an external monitoring tool to monitor Memphis
cover: ../../../.gitbook/assets/Datadog and Memphis.jpeg
coverY: 0
---

# Datadog

## Introduction

As Datadog is one of the popular tools for centralized monitoring, Memphis provides a Prometheus exporter to enable Datadog users to monitor Memphis in K8S deployment only.

## Getting started

### Step 1: Make sure your Memphis Prometheus exporter is on

If you haven't installed Memphis yet - \
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

If Memphis is already installed -

