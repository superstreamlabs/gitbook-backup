---
description: How to configure a Grafana dashboard to visualize Memphis metrics
cover: ../../.gitbook/assets/Grafana + Memphis.jpeg
coverY: 0
---

# Grafana

## Introduction

As Grafana is one of the most popular tools for centralized monitoring, Memphis provides a Prometheus exporter to enable Grafana users to monitor Memphis.

## Prerequisites

* K8S-based Memphis
* Memphis Prometheus exporter
* Configured Prometheus
* Grafana with prometheus configured as a data source

## Getting started

### Step 0: Configuring Prometheus to collect pods' logs

Validate that `Prometheus.yml` configfile contains "kubernetes-pods" job.\
Its mandatory to scrape Memphis exporter metrics automatically.

```yaml
...
honor_labels: true
      job_name: kubernetes-pods
      kubernetes_sd_configs:
      - role: pod
      relabel_configs:
      - action: keep
        regex: true
        source_labels:
        - __meta_kubernetes_pod_annotation_prometheus_io_scrape
...
```

### Step 1: Enabling Memphis Prometheus exporter

**If you haven't** installed Memphis with the `exporter.enabled` yet&#x20;

```
helm install memphis memphis \
--create-namespace --namespace memphis --wait \
--set \
cluster.enabled="true",\
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

### Step 2: Import Memphis dashboard

Import Memphis dashboard using Memphis dashboard ID: **18050**\
[https://grafana.com/grafana/dashboards/18050-memphis/](https://grafana.com/grafana/dashboards/18050-memphis/)

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
