# Overview

## Purpose

Memphis dashboard is designed to provide a quick snapshot of the cluster's health.&#x20;

In one glimpse, the user or operator can check if there are any unhealthy stations, determine the stability of the system components, and monitor system resources.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## System components panel

The "System components" panel had made for the infrastructure administrators to be able to track and monitor virtual resources utilization.

### Fix: "No metrics server found"

In Kubernetes deployments only. If the following warning is shown -

<figure><img src="../.gitbook/assets/Screen Shot 2023-01-30 at 14.22.29.png" alt=""><figcaption></figcaption></figure>

Memphis "System components" panel uses k8s "[metric-server](https://kubernetes-sigs.github.io/metrics-server/)" to read the cluster resources.

#### Installation

#### K8s

The latest Metrics Server release can be installed by running:

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Installation instructions for previous releases can be found in [Metrics Server releases](https://github.com/kubernetes-sigs/metrics-server/releases).

#### minikube

Within minikube enable the metrics-server addon by running:

```
minikube addon enable metrics-server
```

Compatibility matrix:

| Metrics Server | Metrics API group/version | Supported Kubernetes version |
| -------------- | ------------------------- | ---------------------------- |
| 0.6.x          | `metrics.k8s.io/v1beta1`  | \*1.19+                      |
| 0.5.x          | `metrics.k8s.io/v1beta1`  | \*1.8+                       |
| 0.4.x          | `metrics.k8s.io/v1beta1`  | \*1.8+                       |
| 0.3.x          | `metrics.k8s.io/v1beta1`  | 1.8-1.21                     |

\*For <1.16 requires passing `--authorization-always-allow-paths=/livez,/readyz` command line flag
