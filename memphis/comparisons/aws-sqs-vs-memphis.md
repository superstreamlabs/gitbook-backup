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

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## Comparison

| Parameter                  | Memphis                                                                                                | AWS SQS                                                                                          |
| -------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| Performance                | 300K messages per second per station (queue).                                                          | 60K-100K messages per second                                                                     |
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
