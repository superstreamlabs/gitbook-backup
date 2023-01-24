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

### Step 1: Install Datadog

### Step 1: Make sure your Memphis Prometheus exporter is on

If you haven't installed Memphis with the `exporter.enabled` yet -\
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

### Step 2: Add Datadog annotation to Memphis statefulset

Add Datadog annotation to the `memphis-broker` statefulset to expose Prometheus metrics to datadog agent:

```bash
kubectl edit sts memphis-broker -n memphis
```

```yaml
# (...)
spec:
# (...)
  template:
    metadata:
      annotations:
        # (...)
        # Add the following section
        ad.datadoghq.com/metrics.checks: |
           {
             "openmetrics": {
               "instances": [
                 {
                   "openmetrics_endpoint": "http://%%host%%:%%port%%/metrics",
                   "namespace": "memphis",
                   "metrics": [".*"]
                 }
               ]
             }
           }
# (...)
  spec:
    name: metrics
```

Or, in a one-liner command -

```bash
cat <<EOF | kubectl -n memphis patch sts memphis-broker --patch '
spec:
  template:
    metadata:
      annotations:
        ad.datadoghq.com/metrics.checks: |
           {
             "openmetrics": {
               "instances": [
                 {
                   "openmetrics_endpoint": "http://%%host%%:%%port%%/metrics",
                   "namespace": "memphis",
                   "metrics": [".*"]
                 }
               ]
             }
           }'
EOF
```

### Step 3: Check Datadog for Memphis metrics

Reach your Datadog account -> Metrics -> Summary, and check if "memphis" metrics arrives.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-01-24 at 12.14.53.png" alt=""><figcaption></figcaption></figure>

### Step 4: Import the Memphis dashboard

A Datadog [tutorial](https://docs.datadoghq.com/dashboards/#copy-import-or-export-dashboard-json) on how to import a dashboard.

Memphis dashboard .json file to download -

{% embed url="https://raw.githubusercontent.com/memphisdev/gitbook-backup/master/dashboard-gui/integrations/monitoring/MemphisDashboard.json" %}
