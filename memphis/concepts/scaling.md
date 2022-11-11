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

As the workload increases and more data gets transferred through Memphis, more computing resources will be needed to allocated
