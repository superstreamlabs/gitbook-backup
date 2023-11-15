---
description: >-
  This section describes the best practices to run Memphis in Production
  environment and to maximize
---

# Production Best Practices

<details>

<summary>Kubernetes Cluster Requirements</summary>

### Kubernetes version

* Minimum recommended Kubernetes version: 1.24
* Make sure to install a current version of [Kubernetes tools](https://kubernetes.io/docs/tasks/tools/).

### Helm version

* Minimum recommended Helm version: 3.10
* [Install Helm](https://helm.sh/docs/intro/install/)

### Kubernetes Worker Nodes Allocation

To deploy Memphis in a Kubernetes environment, allocate one dedicated worker node for each Memphis broker. This practice serves several vital purposes:

* **Resource isolation:** Memphis assumes that all available system resources are exclusively assigned to Memphis brokers, allowing them to handle substantial data loads and traffic efficiently.
* **Fault tolerance:** Distributing Memphis brokers across separate worker nodes minimizes the impact of a faulty node, ensuring minimal disruption to the entire cluster.
* **High Availability (HA):** Where feasible, consider employing at least two Availability Zones (AZs) and distributing worker nodes across them to enhance high availability.

### CPU

* For production environments, a minimum of 4 CPU cores is recommended.
* Memphis supports both x86\_64 and Arm64 processors.

### Memory

* A minimum of 2 GB of memory per core is required.

### Storage

* **Filesystem:** Utilize the XFS or ext4 filesystem for your storage needs.
* **Dedicated Persistent Volumes:** Assign dedicated Persistent Volumes to each Memphis broker for efficient data storage.
* **Disk Type:** It is recommended to use NVMe disks for storage, while it is advisable to avoid using HDDs.
* **Disk Size:** Allocate a minimum of 50GB disk space per broker. The exact size may vary based on the expected workload, calculated as a function of the "average message size multiplied by the number of messages."

### Security

* Ensure data-at-rest encryption for enhanced security. Additionally, expose the Memphis URLs using HTTPS to protect data in transit.

### Network

* Minimum 10Gbps (10Gige NICs) for optimal performance.

</details>

<details>

<summary>Memphis Cluster Recommendations</summary>

### Memphis Cluster Deployment.

* Deploy a Memphis cluster with a minimum of three replicas. Memphis recommends an odd number of replicas (3, 5, 7, etc.).
* By default, when `global.cluster.enabled="true"` value is provided in the Memphis Helm chart, it deploys:
  * Three Memphis brokers. Users can specify the number of Memphis brokers in the `cluster.replicas` configuration.
  * Two metadata pods with a metadata coordinator.
  * Two RestGW component instances.

### Storage

* Use PersistentVolumes backed by high-performance NVMe disks, which are strongly recommended.
* The Memphis Helm chart uses the default `storageClass` configured in the Kubernetes cluster to create PVCs (Persistent Volume Claims) for each broker. Make sure it is configured.
* &#x20;The default PVC size is 30GB but can be configured using Helm values, for example, `--set memphis.storageEngine.fileStorage.size="100Gi"`.

### Network Configuration

* Memphis requires instances with a minimum of 10Gige NICs for optimal performance.
* Use a LoadBalancer in front of Memphis brokers for UI and WebSocket (WS) ports to enhance the user experience. Load Balancers provide fault tolerance and improve reliability.

#### Internal Network

* Memphis brokers and clients co-located within the same Kubernetes cluster utilize a headless ClusterIP service for their communication. This service is automatically created as part of the Memphis deployment and is associated with the Memphis StatefulSet.
* Any client deployed within the same Kubernetes cluster should employ the internal network with the default internal record: \
  `memphis.<namespace>.svc.cluster.local`.

#### External Network

* External clients, not having access to internal service records, require configuration with externally exposed records.
* Memphis recommends using LoadBalancers to expose its services. [An example yaml file for LoadBalancer configuration.](https://github.com/memphisdev/memphis-k8s/blob/e82b4af54fa48f3f339ca882ccfe57b125e21cdc/examples/external\_svc.yaml)

![](broken-reference)

#### Memphis network architecture

![](broken-reference)

### Secure Memphis&#x20;

* Memphis supports network encryption through TLS. For details, refer to the \[TLS page].
*   Disk encryption, particularly in cloud distributions, can be configured in the `storageClass` configuration. For example:&#x20;

    <pre class="language-yaml"><code class="lang-yaml"><strong>....
    </strong><strong>provisioner: ebs.csi.aws.com
    </strong>parameters:
      csi.storage.k8s.io/fstype: xfs
      encrypted: 'true'
    ....
    </code></pre>

###



</details>

##

## TLDR

1. Use Memphis in cluster mode to enable parallel usage across multiple brokers
2. Spread the workloads across as many partitions as possible (if possible) to spread leaders across different brokers
3. Use NVMe disks for storage.
4. Stretch the retention to days/hours
5. Make sure to utilize as many cores as possible within the app itself. For example, in node.js - use threads.
6. Use affinity rules and separate the client K8s worker from the memphis workers

