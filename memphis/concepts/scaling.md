---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Scaling

This section describes how Memphis scales.

### Scale up vs Scale out

Or vertical vs horizontal are different methods to add more compute/memory/storage resources to a Memphis cluster.

<figure><img src="../../.gitbook/assets/scale up vs out.jpeg" alt=""><figcaption></figcaption></figure>

#### Scale up / Vertical

Addition of CPU / memory / storage to every broker itself (can be only one).\
As mentioned in the hardware requirement table, Memphis can be installed with only one memphis broker over a Kubernetes cluster with a single worker equipped with 2 CPUs, 4Gb of RAM, and 16Gb of storage.

As the workload increases and more data gets transferred through Memphis, more computing resources will be needed to allocate. One way to do it is by strengthening the k8s nodes with more CPUs and based on your stations' storage preferences - adding more memory/storage or both.

Because production-level memphis runs solely on Kubernetes, when done right, it will not affect memphis, and memphis through the entire process will have no downtime at all.

[For more information about upgrading kubernetes workers with zero downtime ](https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-upgrading-your-clusters-with-zero-downtime)

#### Scale-out / Horizontal

{% hint style="info" %}
Relevant for Memphis cluster-mode
{% endhint %}

Scale-out is a concept that exists in distributed applications only.

In such architecture, each cluster node&#x20;
