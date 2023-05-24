---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Scaling

The following section outlines the Memphis scaling process and provides instructions on how to implement it.

## Scaling Up vs Scaling Out / Vertical vs Horizontal Scaling

Memphis utilize the same scaling paradigm as most database or application solutions which is called Scaling Up vs Scaling Out aka Vertical vs Horizontal Scaling.

Scaling up an application refers to increasing the capacity of the current system by adding more resources such as CPU, memory, or storage. This is also known as vertical scaling.

On the other hand, scaling out an application involves distributing the workload across multiple systems or instances to increase capacity. This is also known as horizontal scaling.

Both approaches can be used to increase the capacity of an application to handle a larger workload, but they have different implications and trade-offs. Scaling up a single system can be more cost-effective, but it may reach a physical limit in terms of the resources that can be added. Scaling out requires more infrastructure and may be more complex to set up and manage, but it allows for virtually unlimited scaling potential.

Below, you'll find examples, instructions and best practices on how to scale up/scale up your Memphis cluster.

<figure><img src="../../.gitbook/assets/scale up vs out.jpeg" alt=""><figcaption></figcaption></figure>

### Scaling up / Vertical Scaling

Addition of CPU / memory / storage to every broker itself (can be only one). As mentioned in the hardware requirement table, Memphis can be installed with only one memphis broker over a Kubernetes cluster with a single worker equipped with 2 CPUs, 4GB of RAM, and 16GB of storage. Note that these are the minimum requirements. You can go below them at your own risk but may experience instability in your Memphis ecosystem.

As the demand for computing resources increases with the growth of workload and data transfer through Memphis, it may be necessary to allocate additional resources. One approach to doing this is to increase the number of CPUs in the Kubernetes nodes and, depending on the storage requirements of the system, adding more memory and/or storage capacity.

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

In such a scale type, each cluster node act as a stateless worker of the cluster, and when more power is needed, we add more cluster workers. In Memphis's case, this means adding more Memphis Brokers to handle the increasing workload. Please refer down below for a example on how to add more Memphis brokers.

#### A step-by-step guide to adding more Memphis brokers

Step 1: Add more StatefulSets

```
kubectl scale statefulsets memphis-broker --replicas=<new amount of replicas> -n memphis
```

Step 2: Edit Memphis ConfigMap route table

```
kubectl get cm memphis-broker-config -o yaml > memphis-broker-config.yaml
vi memphis-broker-config.yaml
```

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-13 at 17.08.25.png" alt=""><figcaption></figcaption></figure>

Add the new StatefulSet in the marked line with the following pattern -

`, nats://memphis-broker-`<mark style="color:red;">**`X`**</mark>`.memphis-cluster.memphis.svc.cluster.local:6222`
