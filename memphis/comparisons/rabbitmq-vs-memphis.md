---
description: This section describes the differences between RabbitMQ and Memphis
cover: ../../.gitbook/assets/RabbitMQ vs Memphis.jpeg
coverY: 0
---

# RabbitMQ vs Memphis

## **What is RabbitMQ?**

RabbitMQ is a lightweight and easy-to-deploy messaging queue for on-premises and cloud environments. It supports multiple messaging protocols. RabbitMQ can be deployed in distributed and federated configurations to meet high-scale, high-availability requirements.

## **What is Memphis.dev?**

**Memphis** is a next-generation messaging queue.

A simple, robust, and durable cloud-native message broker wrapped with an entire ecosystem that enables fast and reliable development of next-generation event-driven use cases.

Memphis.dev enables building next-generation applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.



<figure><img src="../../.gitbook/assets/memphis vs legacy.jpeg" alt=""><figcaption></figcaption></figure>

## Messaging Comparison

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
| Multi-region       | Supported\*                                                                                            | No                                                                                                    |
| Read-replicas      | Supported\*                                                                                            | No                                                                                                    |
| Queue striping     | Supported\*                                                                                            | No                                                                                                    |

### **Data Flow**&#x20;

RabbitMQ uses a distinct, bounded data flow. Messages are created and sent by the producer and received by the consumer. Memphis uses an unbounded data flow, with the key-value pairs continuously streaming to the assigned station.

### **Data Usage**

RabbitMQ is best for transactional data, such as order formation, placement, and user requests. Memphis works great for transactional and operational data like process operations, auditing and logging statistics, and system activity.

### **Message retention**&#x20;

RabbitMQ pushes messages to consumers. These messages are removed from the queue once they are processed and acknowledged. Memphis is a log. It uses continuous messages, which stay in the station (queue) until the retention period expires.

### **Topology**&#x20;

RabbitMQ uses the exchange queue topology — sending messages to an exchange where they are, in turn, routed to various queue bindings for the consumer’s use. Memphis employs the publish/subscribe topology, sending messages across the streams to the correct stations, and then consumed by users in the different authorized groups.

### **Architecture Differences**

When choosing between Memphis and RabbitMQ, the internal operations and fundamental design can be essential considerations.&#x20;

The components of RabbitMQ’s Architecture consist of the following:

* Queue: It is in charge of keeping track of messages that have been received and may have configuration data that specifies what it can do with a message.
* Exchange: An exchange receives messages sent to RabbitMQ and determines where they should be forwarded. Exchanges define the routing strategies used for messages, most frequently by examining the data characteristics transmitted with the message or included inside its attributes.
* Producer: Produces messages and sends them to a broker server (publishes). A payload and a label are the two components of a message. The user's desired data to convey is the payload. The label specifies who should receive a copy of the message and describes the payload.
* Consumer: It subscribes to a queue and is connected to a broker server.&#x20;
* Broker: Applications can exchange information and communicate with one another through a broker.
* Binding: It tells an exchange which queues to distribute messages. Additionally, the binding will instruct the exchange to filter which messages it is permitted to add to a queue for specific exchange types.

Memphis architecture is designed using the following components:

* Replicas: One of the most crucial elements of Memphis is replication, which ensures that messages are published and consumed even when the broker encounters a problem.
* RAFT: It maintains data coordination between brokers such as configuration, location, data, and status details.&#x20;
* Producer: Producers push or publish messages to a Memphis station created on a Memphis broker. Producers can also send messages to a broker synchronously or asynchronously.&#x20;
* Consumers: Individuals who subscribe to a Memphis station and pull messages from it. Memphis allows data to be stored in additional storage platforms used by programs for online transaction processing (OLTP) beside internal disks and memory.&#x20;
* Broker: Acts as a Memphis server, or broker. The number of streams for each message is defined in accordance with the order in which the broker stores the messages.

### **Scalability and Redundancy**

RabbitMQ uses a round-robin queue to repeat messages. To boost throughput and balance the load, the messages are divided among the queues. Additionally, it enables numerous consumers to read messages from various queues at once.

In Memphis, scalability and redundancy are provided by stream objects. The stream is duplicated across numerous brokers. In the event that one of the brokers fails, the data can still be served by another broker.

If data is stored in only one broker, the dependence on that broker will grow, which is hazardous and increases the likelihood that it will fail. Additionally, distributing the streams will vastly improve throughput.

### Multi-region/Multi-cloud

The highest availability capabaility exist in RabbitMQ is cluster and quorom queues which enable cross nodes replication of queues.

Besides mirroring and striping station across multiple brokers that can ultimatly span across AZs, partners and Memphis cloud users can establish a memphis super cluster across regions in an active/passive topology.

### Queue striping

All data/state required for the operation of a RabbitMQ broker is replicated across all nodes. An exception to this are message queues, which by default reside on one node, though they are visible and reachable from all nodes. To replicate queues across nodes in a cluster, use a queue type that supports replication. This topic is covered in the [Quorum Queues](https://www.rabbitmq.com/quorum-queues.html) guide.

Memphis replicates data across brokers based on defined policy (Replicas). Memphis station can also span and stripe data across brokers to achieve higher throughput and availability.

## Features Comparison

| Parameter                          | Memphis                            | RabbitMQ     |
| ---------------------------------- | ---------------------------------- | ------------ |
| GUI                                | True                               | True         |
| Schema Management                  | True                               | False        |
| Inline stream enrichment           | True                               | False        |
| Ready-to-use connectors            | True                               | False        |
| Real-time message tracing          | True                               | False        |
| Data-Level Observability           | True                               | True         |
| Automatic environment optimization | True                               | False        |
| Deduplication                      | True. Modified bloom filter        | False        |
| Dead-letter                        | True                               | True         |
| REST Gateway                       | True                               | False        |
| Consumer internal communication    | Experimental                       | False        |
| Production deployment environment  | Kubernetes                         | Bare-metal   |
| Storage tiering                    | Disk, Memory, **S3 for Archiving** | Disk, Memory |

### Libraries and Language Support

RabbitMQ supports Python, Ruby, Elixir, PHP, Swift, Go, Java, C, Spring, .Net, and JavaScript.

Memphis supports Node js, Python, Go, .NET, Java, NestJS, and Typescript.\
