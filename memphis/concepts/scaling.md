---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Scaling

This section describes how Memphis scales.

## Scale up vs Scale out

Or vertical vs horizontal are different methods to add more compute/memory/storage resources to a Memphis cluster.

<figure><img src="../../.gitbook/assets/scale up vs out.jpeg" alt=""><figcaption></figcaption></figure>

### Scale up / Vertical

Addition of CPU / memory / storage to every broker itself (can be only one).\
As mentioned in the hardware requirement table, Memphis can be installed with only one memphis broker over a Kubernetes cluster with a single worker equipped with 2 CPUs, 4Gb of RAM, and 16Gb of storage.

As the workload increases and more data gets transferred through Memphis, more computing resources will be needed to allocate. One way to do it is by strengthening the k8s nodes with more CPUs and based on your stations' storage preferences - adding more memory/storage or both.

Because production-level memphis runs solely on Kubernetes, when done right, it will not affect memphis, and memphis through the entire process will have no downtime at all.

[For more information about upgrading kubernetes workers with zero downtime ](https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-upgrading-your-clusters-with-zero-downtime)

#### Strengths

* **Relative speed:** Replacing a resource, such as a single processor, with a dual processor, means that the throughput of the CPU is doubled. The same can be done to resources such as dynamic random access memory (DRAM) to improve dynamic memory performance.
* **Simplicity:** Increasing the size of an existing system means that network connectivity and software configuration do not change. As a result, the time and effort saved to ensure the scaling process is much more straightforward than scaling out architecture.

### Scale-out / Horizontal

{% hint style="info" %}
Relevant for Memphis [cluster-mode](https://docs.memphis.dev/memphis/deployment/kubernetes#step-1-installation)
{% endhint %}

Scale-out is a concept that exists in distributed applications only.

In such a scale type, each cluster node act as a stateless worker of the cluster, and when more power is needed, we add more cluster workers.

