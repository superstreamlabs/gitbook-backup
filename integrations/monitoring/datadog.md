---
description: Use Datadog as an external monitoring tool to monitor Memphis
cover: ../../.gitbook/assets/Datadog and Memphis.jpeg
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

**If you haven't** installed Memphis with the `exporter.enabled` yet&#x20;

```
helm install memphis memphis/memphis \
--create-namespace --namespace memphis --wait \
--set \
global.cluster.enabled="true",\
exporter.enabled="true"
```

**If Memphis is already installed -**

#### Obtain the credentials used to hold the Metadata data on your current release:

```bash
export PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata -o jsonpath="{.data.password}" | base64 -d)
export REPMGR_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata -o jsonpath="{.data.repmgr-password}" | base64 -d)
export ADMIN_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata-coordinator -o jsonpath="{.data.admin-password}" | base64 -d)
```

#### Use helm upgrade to add exporter to the deployment:

```
helm upgrade memphis memphis/memphis -n memphis --set exporter.enabled=true,metadata.postgresql.password=$PASSWORD,metadata.postgresql.repmgrPassword=$REPMGR_PASSWORD,metadata.pgpool.adminPassword=$ADMIN_PASSWORD
```

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

<figure><img src="../../.gitbook/assets/Screenshot 2023-01-24 at 12.14.53.png" alt=""><figcaption></figcaption></figure>

### Step 4: Import the Memphis dashboard

A Datadog [tutorial](https://docs.datadoghq.com/dashboards/#copy-import-or-export-dashboard-json) on how to import a dashboard.

Memphis dashboard .json file to download -

{% embed url="https://raw.githubusercontent.com/memphisdev/gitbook-backup/master/dashboard-gui/integrations/monitoring/MemphisDashboard.json" %}
