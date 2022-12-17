---
description: This section describes the differeneces between RabbitMQ and Memphis
---

# RabbitMQ vs Memphis

## **What is RabbitMQ?**

RabbitMQ is a lightweight and easy-to-deploy messaging queue for on-premises and cloud environments. It supports multiple messaging protocols. RabbitMQ can be deployed in distributed and federated configurations to meet high-scale, high-availability requirements.

## **What is Memphis.dev?**

**Memphis** is the next generation of messaging queues.

A simple, robust, and durable cloud-native message broker wrapped with an entire ecosystem that enables fast and reliable development of next-generation event-driven use cases.

Memphis.dev enables building next-generation applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.



<figure><img src="../../.gitbook/assets/memphis vs legacy.jpeg" alt=""><figcaption></figcaption></figure>

## Messaging Comparison Table

| Parameter          | Memphis                                                                                                | RabbitMQ                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| Performance        | 300K messages per second per station (queue).                                                          | 4K-10K messages per second                                                                            |
| Message Retention  | Policy-based (e.g., 30 days)                                                                           | Acknowledgment based                                                                                  |
| Data Type          | Operational                                                                                            | Transactional                                                                                         |
| Consumer Mode      | Smart broker/Smart consumer                                                                            | Smart broker/dumb consumer                                                                            |
| Topology           | Exchange type: Direct, Fan out, Topic, Header-based                                                    | Publish/subscribe based                                                                               |
| Payload Size       | Up to 15M                                                                                              | No constraints                                                                                        |
| Use Cases          | Massive data/high throughput cases \| Simple use cases                                                 | Simple use cases                                                                                      |
| Delivery Guarantee | At least once, Exactly once                                                                            | Especially in relation to transactions utilizing a single queue, it does not guarantee atomicity.     |
| Message ordering   | Message ordering is provided via consumer groups. By message key, messages are sent to stations.       | Unsupported.                                                                                          |
| Message priorities | Unavailable                                                                                            | You can set message priorities in RabbitMQ and consume messages in the order of highest priority.     |
| Message lifetime   | Since station messages are kept on file/memory. This can be controlled by defining a retention policy. | Because RabbitMQ is a queue, messages are discarded after being read, and an acknowledgment is given. |
| Clustering         | Active-Active                                                                                          | Active-Passive                                                                                        |

