---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Scaling for Live/Production Deployment

This section describes how Memphis scales.

## Scaling Up vs Scaling Out

Or vertical vs horizontal are different methods to add more compute/memory/storage resources to a Memphis cluster.

<figure><img src="../../.gitbook/assets/scale up vs out.jpeg" alt=""><figcaption></figcaption></figure>

### Scaling up / Vertical Scaling

Addition of CPU / memory / storage to every broker itself (can be only one).
As mentioned in the hardware requirement table, Memphis can be installed with only one memphis broker over a Kubernetes cluster with a single worker equipped with 2 CPUs, 4Gb of RAM, and 16Gb of storage.

As the demand for computing resources increases with the growth of workload and data transfer through Memphis, it may be necessary to allocate additional resources. One approach to doing this is to increase the number of CPUs in the k8s nodes and, depending on the storage requirements of the system, adding more memory and/or storage capacity.

As long as it is implemented correctly, using Kubernetes for the production-level of Memphis will not impact the functioning of Memphis and there will be no downtime during the scaling process.

[For further information on upgrading Kubernetes nodes with zero downtime](https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-upgrading-your-clusters-with-zero-downtime)

#### Strengths

* **Relative speed:** Upgrading a resource, such as replacing a single processor with a dual processor, doubles the processing speed of the CPU. Similarly, upgrading dynamic random access memory (DRAM) can enhance the dynamic memory performance of the system.
* **Simplicity:** Expanding the size of a current system does not alter network connectivity or software configuration, making the scaling process more efficient and requiring less time and effort compared to the alternative of scaling out the architecture.

### Scaling out / Horizontal Scaling

{% hint style="info" %}
Relevant for Memphis [cluster-mode](https://docs.memphis.dev/memphis/deployment/kubernetes#step-1-installation)
{% endhint %}

Scale-out is a concept that exists in distributed applications only.

In such a scale type, each cluster node act as a stateless worker of the cluster, and when more power is needed, we add more cluster workers.

#### A step-by-step guide to adding more memphis brokers

Step 1: Add more statefulset

```
kubectl scale statefulsets memphis-broker --replicas=<new amount of replicas> -n memphis
```

Step 2: Edit Memphis configmap route table

```
kubectl get cm memphis-broker-config -o yaml > memphis-broker-config.yaml
```

vi `memphis-broker-config.yaml`

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-13 at 17.08.25.png" alt=""><figcaption></figcaption></figure>

Add the new statefulset in the marked line with the following pattern -&#x20;

`, nats://memphis-broker-`<mark style="color:red;">**`X`**</mark>`.memphis-cluster.memphis.svc.cluster.local:6222`
