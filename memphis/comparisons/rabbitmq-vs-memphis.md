---
description: This section describes the differeneces between RabbitMQ and Memphis
cover: ../../.gitbook/assets/RabbitMQ vs Memphis.jpeg
coverY: 0
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
| Data Type          | Transactional, Operational                                                                             | Transactional                                                                                         |
| Consumer Mode      | Smart broker/Smart consumer                                                                            | Smart broker/dumb consumer                                                                            |
| Topology           | Exchange type: Direct, Fan out, Topic, Header-based                                                    | Publish/subscribe based                                                                               |
| Payload Size       | Up to 15M                                                                                              | No constraints                                                                                        |
| Use Cases          | Massive data/high throughput cases \| Simple use cases                                                 | Simple use cases                                                                                      |
| Delivery Guarantee | At least once, Exactly once                                                                            | Especially in relation to transactions utilizing a single queue, it does not guarantee atomicity.     |
| Message ordering   | Message ordering is provided via consumer groups. By message key, messages are sent to stations.       | Unsupported.                                                                                          |
| Message priorities | Unavailable                                                                                            | You can set message priorities in RabbitMQ and consume messages in the order of highest priority.     |
| Message lifetime   | Since station messages are kept on file/memory. This can be controlled by defining a retention policy. | Because RabbitMQ is a queue, messages are discarded after being read, and an acknowledgment is given. |
| Clustering         | Active-Active                                                                                          | Active-Passive                                                                                        |

**Data Flow**&#x20;

RabbitMQ uses a distinct, bounded data flow. Messages are created and sent by the producer and received by the consumer. Memphis uses an unbounded data flow, with the key-value pairs continuously streaming to the assigned station.

**Data Usage**

RabbitMQ is best for transactional data, such as order formation, placement, and user requests. Memphis works great for transactional and operational data like process operations, auditing and logging statistics, and system activity.

**Message retention**&#x20;

RabbitMQ pushes messages to consumers. These messages are removed from the queue once they are processed and acknowledged. Memphis is a log. It uses continuous messages, which stay in the station (queue) until the retention period expires.

**Topology**&#x20;

RabbitMQ uses the exchange queue topology — sending messages to an exchange where they are, in turn, routed to various queue bindings for the consumer’s use. Memphis employs the publish/subscribe topology, sending messages across the streams to the correct stations, and then consumed by users in the different authorized groups.

**Architecture Differences**

When choosing between Memphis and RabbitMQ, the internal operations and fundamental design can be essential considerations.&#x20;

The components of RabbitMQ’s Architecture consist of the following:

* Queue: It is in charge of keeping track of messages that have been received and may have configuration data that specifies what it can do with a message.
* Exchange: An exchange receives messages sent to RabbitMQ and determines where they should be forwarded. Exchanges define the routing strategies that are used for messages, most frequently by examining the data characteristics that are transmitted with the message or included inside its attributes.
* Producer: Produces messages and sends them to a broker server (publishes). A payload and a label are the two components of a message. The user's desired data to convey is the payload. The label specifies who should receive a copy of the message and describes the payload.
* Consumer: It subscribes to a queue and is connected to a broker server.&#x20;
* Broker: Applications can exchange information and communicate with one another through a broker.
* Binding: It tells an exchange which queues to distribute messages. Additionally, the binding will instruct the exchange to filter which messages it is permitted to add to a queue for specific exchange types.

\
