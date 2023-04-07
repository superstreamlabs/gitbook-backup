---
description: This section describes the differences between AWS SQS and Memphis
cover: ../../.gitbook/assets/AWS SQS vs Memphis.jpeg
coverY: 0
---

# AWS SQS vs Memphis

## What is AWS SQS?

Amazon Simple Queue Service (Amazon SQS) offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components. Amazon SQS offers common constructs such as dead-letter queues and cost allocation tags. It provides a generic web services API that you can access using any programming language that the AWS SDK supports.

## **What is Memphis.dev?**

**Memphis** is a next-generation messaging queue.

A simple, robust, and durable cloud-native message broker wrapped with an entire ecosystem that enables fast and reliable development of next-generation event-driven use cases.

Memphis.dev enables building next-generation applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

## Messaging

| Parameter                  | Memphis                                                                                                | AWS SQS                                                                                          |
| -------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| Benchmark                  | 300K messages per second per station (queue).                                                          | 60K-100K messages per second                                                                     |
| Message Retention          | Policy-based (e.g., 30 days)                                                                           | Acknowledgment based                                                                             |
| Data Type                  | Transactional, Operational                                                                             | Transactional                                                                                    |
| Consumer Mode              | Smart broker/Smart consumer                                                                            | Smart broker/dumb consumer                                                                       |
| Topology                   | Publish/subscribe based                                                                                | Not supporting broadcasting. 1:1 producer to subscriber                                          |
| Payload Size               | Up to 15M                                                                                              | Max of 256KB                                                                                     |
| Batch size                 | No limitation                                                                                          | Max of 10 messages                                                                               |
| Use Cases                  | Massive data/high throughput cases \| Simple use cases                                                 | Simple use cases                                                                                 |
| Delivery Guarantee         | At least once, Exactly once                                                                            | At least once                                                                                    |
| Message ordering           | Message ordering is provided via consumer groups. By message key, messages are sent to stations.       | FIFO is optional                                                                                 |
| Message priorities         | Unavailable                                                                                            | Unavailable                                                                                      |
| Message lifetime           | Since station messages are kept on file/memory. This can be controlled by defining a retention policy. | Because SQS is a queue, messages are discarded after being read, and an acknowledgment is given. |
| Clustering                 | Active-Active                                                                                          | Unavailable                                                                                      |
| Performance                | Scale-up, Scale-out                                                                                    | No control                                                                                       |
| Multi-region               | Supported\*                                                                                            | No                                                                                               |
| Multi-tenancy              | Supported\*                                                                                            | No                                                                                               |
| Read-replicas              | Supported\*                                                                                            | No                                                                                               |
| Data striping across nodes | Supported                                                                                              | Unavailable                                                                                      |

\*Available for Memphis cloud users

### **Data Flow**&#x20;

SQS uses a distinct, bounded data flow. Messages are created and sent by the producer and received by the consumer.&#x20;

Memphis uses an unbounded data flow, with the key-value pairs continuously streaming to the assigned station.

### **Data Usage**

AWS SQS is best for transactional data, such as order formation, placement, and user requests.&#x20;

Memphis works great for transactional and operational data like process operations, auditing and logging statistics, and system activity.

### **Message retention**&#x20;

AWS SQS pushes messages to consumers. These messages are removed from the queue once they are processed and acknowledged.&#x20;

Memphis is a log. It uses continuous messages, which stay in the station (queue) until the retention period expires.

### Multi-tenancy

**AWS SQS** doesn't support multi-tenancy but through a lambda function, required to be code and managed by the user that acts as a router.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.36.37.png" alt=""><figcaption><p>AWS SQS</p></figcaption></figure>

**Memphis** supports multi-tenancy using namespaces which offers a complete separation from connections, producers, consumers, security, dedicated dashboard, including node selection.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.10.43.png" alt=""><figcaption><p>Memphis namespaces</p></figcaption></figure>

### **Observability**&#x20;

Some level of observability can be received by using 3rd party apps like Cloudwatch/Datadog/New Relic. To understand the full path of a message, it is required to use AWS X-Ray and add some headers to each client. Notifications can be achieved by building a dedicated event queue with lambda triggers. Some alarms and triggers must be defined over 3rd party apps to enable lag identifications and latency in real-time.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.38.55.png" alt=""><figcaption></figcaption></figure>

Memphis offers full Infra-to-cluster-to-data GUI-based observability, monitoring, real-time message tracing, and notifications embedded inside the management layer, including self-healing policies based on the defined events.\


<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.26.04.png" alt=""><figcaption><p>Memphis GUI</p></figcaption></figure>

<div>

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.26.51.png" alt=""><figcaption><p>Troubleshooting process</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.26.36.png" alt=""><figcaption><p>Notification Center</p></figcaption></figure>

</div>

## Features

| Parameter                            | Memphis.dev                                             | AWS SQS                                               |
| ------------------------------------ | ------------------------------------------------------- | ----------------------------------------------------- |
| GUI                                  | Yes                                                     | Yes                                                   |
| Schema Management                    | Yes                                                     | No                                                    |
| Wildcard consume                     | No                                                      | Yes                                                   |
| Stream Enrichment                    | Yes                                                     | Yes                                                   |
| Ready-to-use source/sinks connectors | Yes                                                     | No                                                    |
| Stream lineage                       | Yes                                                     | No                                                    |
| Data-Level Observability             | Yes                                                     | Yes                                                   |
| Self-healing                         | Yes + Managed service                                   | Managed service                                       |
| Deduplication                        | Yes. Modified bloom filter                              | Deduplication interval of 5 minutes                   |
| Delayed queues                       | Yes. Atomic per message.                                | Yes. Not atomic, and per entire queue.                |
| Dead-letter                          | Yes                                                     | Yes                                                   |
| REST Gateway                         | Yes                                                     | No                                                    |
| Consumer internal communication      | Experimental                                            | No                                                    |
| Production deployment environment    | Kubernetes, Docker, Managed service                     | Managed service                                       |
| Storage tiering                      | Disk, Memory, **S3 for Archiving**                      | Disk                                                  |
| Notifications                        | Slack, Email, More                                      | With SNS and Cloudwatch                               |
| SDK support                          | Node js, Python, Go, .NET, Java, NestJS, and Typescript | C++, Go, Java, .NET, Python, node.js, Rust, Ruby, PHP |

## Performance comparison

**AWS SQS** Client node: 1 x m4.2xlarge / 50 threads

**Memphis** Client node: 1 x m5n.8xlarge / 20 threads

| 1KB Messages          | Memphis | AWS SQS |
| --------------------- | ------- | ------- |
| 100K messages = 100MB |         |         |
| 500K messages = 500MB |         |         |
| 1M messages = 1GB     |         |         |
| 10M messages = 10GB   |         |         |

## TCO comparison

### Defining a cost model for data streaming

In this economic climate, costs are top of mind for everyone.

Total Cost of Ownership (TCO) should be a primary consideration when evaluating the Return on Investment (ROI) of adopting a new software platform. TCO is the blended cost of deploying, configuring, securing, productionizing, and operating the software over its expected lifetime, including all infrastructure, personnel, training, and subscription costs.&#x20;

For this comparison, we define TCO as a combination of the following components:

1. Implementation: The cost of implementing a new streaming technology
2. Infrastructure: The cost of computing and storage, in this case on AWS

For the infrastructure cost comparison, we ran benchmarks to compare the performance of AWS SQS against Memphis.

### Implementation costs

Oftentimes, there is a misconception that cloud services are a turnkey solution. \
Here are some of the missing components that will need to be constructed when using AWS SQS -&#x20;

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td></td><td>Performance heavily relies on the client’s threads</td><td></td></tr><tr><td></td><td>What if you required to run on GCP for specific customer?</td><td></td></tr><tr><td></td><td><p>Not built for SaaS.</p><p>No multi-tenancy</p></td><td></td></tr><tr><td></td><td>Consumer-side delay queues</td><td></td></tr><tr><td></td><td>Monitoring and notification</td><td></td></tr><tr><td></td><td>Consumption from DLQ</td><td></td></tr></tbody></table>

### Implementation costs

| Feature                      | Memphis                                                                                                                                                                     | AWS SQS                                                                                                                                                    |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Performance                  | <p>Built-in. Automatic.</p><p><mark style="color:purple;"><strong>0 dev hours</strong></mark></p>                                                                           | <p>Required to use threads.</p><p><mark style="color:orange;"><strong>130 dev hours</strong></mark></p>                                                    |
| Multi-Cloud                  | <p>Built-in. By design.</p><p><mark style="color:purple;"><strong>0 dev hours</strong></mark></p>                                                                           | <p>Required to build abstraction to different cloud queues and APIs.<br><mark style="color:orange;"><strong>378 dev hours</strong></mark></p>              |
| Multi-tenancy                | <p>Built-in. By design.</p><p><mark style="color:purple;"><strong>0 dev hours</strong></mark></p>                                                                           | <p>Required to build. Using different queues and/or tagging data.</p><p><mark style="color:orange;"><strong>63 dev hours</strong></mark></p>               |
| Monitoring and Notifications | <p>Built-in. Ready-to-use slack notifications / Grafana / Datadog / prometheus.</p><p><mark style="color:purple;"><strong>12 dev hours + 2 DevOps hours</strong></mark></p> | <p>Required to build + use 3rd party open-source/paid tools.</p><p><mark style="color:orange;"><strong>126 DevOps hours + 49 dev hours</strong></mark></p> |
| Runtime DLQ consumption      | <p>Built-in.</p><p><mark style="color:purple;"><strong>0 dev hours</strong></mark></p>                                                                                      | <p>Required to build.</p><p><mark style="color:orange;"><strong>30 dev hours</strong></mark></p>                                                           |
| Delayed consumers            | <p>Built-in.</p><p><mark style="color:purple;"><strong>0 dev hours</strong></mark></p>                                                                                      | <p>Required to build.</p><p><mark style="color:orange;"><strong>20 dev hours</strong></mark></p>                                                           |
| Cost                         | <mark style="color:purple;">**$1,000**</mark>                                                                                                                               | <p>Based on average dev hourly rate of $70.</p><p><mark style="color:orange;"><strong>$55,720 ($54,720 difference)</strong></mark></p>                     |

