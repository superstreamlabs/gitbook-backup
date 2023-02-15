---
description: How to configure a Grafana dashboard to monitor Memphis
cover: ../../../.gitbook/assets/Grafana + Memphis (1).jpeg
coverY: 0
---

# Grafana

## Introduction

As Grafana is one of the most popular tools for centralized monitoring, Memphis provides a Prometheus exporter to enable Grafana users to monitor Memphis.

## Getting started

### Step 1: Make sure your Memphis Prometheus exporter is on

**If you haven't** installed Memphis with the `exporter.enabled` yet -\
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

**If Memphis is already installed -**

```
helm upgrade --set exporter.enabled=true memphis --namespace memphis --reuse-values
```

