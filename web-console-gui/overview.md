---
description: This section describes the different components in the dashboard
cover: ../.gitbook/assets/Web Console.jpeg
coverY: 0
---

# Dashboard

## Introduction

Memphis dashboard is designed to provide a quick snapshot of the platform's health.&#x20;

Operators can check if there are any unhealthy stations, determine the general status of the system, and in the case of a self-hosted Memphis - monitor system resources.

{% tabs %}
{% tab title="Open-source" %}
<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Total stations

The total amount of stations across the platform.

### Total messages

The total amount of messages across the different stations.

### Total dead-letter

The total amount of dead-letter messages across the different stations.

### Live throughput

The total read/write throughput of the system over the span of 10 minutes.

### System components panel

The "System components" panel had made for the infrastructure administrators to be able to track and monitor virtual resources utilization.

#### Fix: "No metrics server found"

In <mark style="color:red;">Kubernetes deployments only</mark>. If the following warning is shown -

<figure><img src="../.gitbook/assets/Screen Shot 2023-01-30 at 14.22.29 (1).png" alt=""><figcaption></figcaption></figure>

System components use K8S "[metric-server](https://kubernetes-sigs.github.io/metrics-server/)" to pull cluster resources.

#### Here is how to install it in a non-Minikube environment

The latest Metrics Server release can be installed by running the following:

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Installation instructions for previous releases can be found in [Metrics Server releases](https://github.com/kubernetes-sigs/metrics-server/releases).

#### Here is how to install it in a Minikube environment

Within Minikube, enable the metrics-server addon by running the following:

```
minikube addons enable metrics-server
```

Compatibility matrix:

<table><thead><tr><th width="165.33333333333331">Metrics Server</th><th>Metrics API group/version</th><th>Supported Kubernetes version</th></tr></thead><tbody><tr><td>0.6.x</td><td><code>metrics.k8s.io/v1beta1</code></td><td>*1.19+</td></tr><tr><td>0.5.x</td><td><code>metrics.k8s.io/v1beta1</code></td><td>*1.8+</td></tr><tr><td>0.4.x</td><td><code>metrics.k8s.io/v1beta1</code></td><td>*1.8+</td></tr><tr><td>0.3.x</td><td><code>metrics.k8s.io/v1beta1</code></td><td>1.8-1.21</td></tr></tbody></table>

\*For <1.16 requires passing `--authorization-always-allow-paths=/livez,/readyz` command line flag
{% endtab %}

{% tab title="Cloud" %}
<figure><img src="../.gitbook/assets/Screen Shot 2023-06-28 at 13.23.27.png" alt=""><figcaption></figcaption></figure>

### Stations

The total amount of existing stations across the platform.

### Slow consumption station

Stations with a growing slowness of consumption vs production rate. Meaning that in specific stations, producers produce messages much faster than the consumers are able to consume, and a delay is starting the get created.

### Total dead-letter

The total amount of dead-letter messages across the different stations.

### Live throughput

The total read/write throughput of the system over the span of 10 minutes.
{% endtab %}
{% endtabs %}
