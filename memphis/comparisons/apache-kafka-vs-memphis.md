---
description: This section describes the differeneces between Apache Kafka and Memphis
cover: ../../.gitbook/assets/Kafka vs Memphis.dev.jpeg
coverY: 0
---

# Apache Kafka vs Memphis

## What is Apache Kafka?

Apache Kafka is an open-source distributed event streaming platform. Based on the abstraction of a distributed commit log, Kafka is capable of handling a great amount of events with functionality comprising pub/sub.

## **What is Memphis.dev?**

**Memphis** is a next-generation messaging queue.

A simple, robust, and durable cloud-native message broker wrapped with an entire ecosystem that enables fast and reliable development of next-generation event-driven use cases.

Memphis.dev enables building next-generation applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.

## General

| Parameter                 | Memphis.dev                              | Apache Kafka                           |
| ------------------------- | ---------------------------------------- | -------------------------------------- |
| License                   | Apache 2.0                               | Apache 2.0                             |
| Components                | Memphis + MongoDB (MDB is being removed) | Kafka + Zookeeper(ZK is being removed) |
| Message consumption model | Pull                                     | Pull                                   |
| Storage architecture      | Log                                      | Log                                    |

### License

Both technologies are available under fully open-source licenses. Memphis also has a Memphis-based distribution with added security, tiered storage, and more.

### Components

Kafka uses Apache Zookeeper™ for consensus and message storage.\
Memphis uses MongoDB for GUI state management only and will be removed soon, making Memphis without any external dependency. Memphis achieves consensus by using RAFT.

### Message Consumption Model

Both Kafka and Memphis use a pull-based architecture where consumers pull messages from the server, and [long-polling](https://en.wikipedia.org/wiki/Push\_technology#Long\_polling) is used to ensure new messages are made available instantaneously.

Pull-based architectures are often preferable for high throughput workloads as they allow consumers to manage their flow control, fetching only what they need.

### Storage Architecture

Kafka uses a distributed commit log as its storage layer. Writes are appended to the end of the log. Reads are sequential, starting from an offset, and data is zero-copied from the disk buffer to the network buffer. This works well for event streaming use cases.

Memphis also uses a distributed commit log called streams (made by NATS Jetstream) as its storage layer, which can be written entirely on the broker's (server) memory or disk.\
Memphis also uses offsets but abstracts them completely, so the heavy lifting of saving a record of the used offsets resides on Memphis and not on the client.\
Memphis also offers storage tiering for offloading messages to S3-compatible storage for an infinite storage time and more cost-effective storage. Reads are sequential.

## Ecosystem and User Experience

| Parameter                            | Memphis.dev               | Apache Kafka                           |
| ------------------------------------ | ------------------------- | -------------------------------------- |
| Deployment                           | Stright forward           | Requires deep understanding and design |
| Enterprise support                   | Yes                       | 3rd parties like Confluent, AWS MSK    |
| Managed cloud offerings              | Yes                       | 3rd parties like Confluent, AWS MSK    |
| Self-Healing                         | Yes                       | No                                     |
| Notifications                        | Yes                       | No                                     |
| Message tracing (aka Stream lineage) | Yes                       | No                                     |
| Storage balancing                    | Automatic based on policy | Manual                                 |

### Deployment

Kafka is a cluster-based technology with a medium-weight architecture requiring two distributed components: Kafka's own servers (brokers) plus ZooKeeper™ servers. Zookeeper adds an additional level of complexity but the community is in the process of removing the ZooKeeper component from Kafka. Kafka "Vanilla" deployment requires a manual binary installation and text-based configuration, as well as config OS daemons and internal parameters.

Memphis has a light-weight yet robust cloud-native architecture and packed as a container from day one. It can be deployed using any docker engine, docker swarm, and for production environment using helm for Kubernetes (soon with operator). Memphis initial config is already sufficient for production, and optimizations can take place on-the-fly without downtime. That approach enables Memphis to be completely isolated and apart from the infrastructure it deployed upon.

### Enterprise support and managed cloud offering

Enterprise-grade support and managed cloud offerings for Kafka are available from several prominent vendors, including Confluent, AWS (MSK), Cloudera, and more.

Memphis provides enterprise support and managed cloud offering that includes features like enhanced security, stream research abilities, an ML-based resource scheduler for better cost savings, and more.

### Self-healing

Kafka is a robust distributed system and requires constant tune-ups, client-made wrappers, management, and tight monitoring. The user or operator is responsible for ensuring it's alive and works as required. This approach has pros and cons, as the user can tune almost every parameter, which is often revealed as a significant burden.

One of Memphis' core features is to remove frictions of management and autonomously make sure it's alive and performing well using periodic self-checks and proactive rebalancing tasks, as well as fencing the users from misusing the system. In parallel, every aspect of the system can be configured on-the-fly without downtime.

### Notifications

Memphis has a built-in notification center that can push real-time alerts based on defined triggers like client disconnections, resource depletion, schema violation, and more.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.26.36.png" alt=""><figcaption></figcaption></figure>

Apache Kafka does not offer an embedded solution for notifications. Can be achieved via commercial offerings.

### Message tracing (aka Stream lineage)

Tracking stream lineage is the ability to understand the full path of a message from the very first producer through the final consumer, including the trail and evolvement of a message between topics. This ability is extremely handy in a troubleshooting process.

Apache Kafka does not provide a native ability for stream lineage, but it can be achieved using OpenTelemetry or OpenLineage frameworks, as well as integrating 3rd party applications such as datadog, epsagon, or using Confluent's cloud offering.

Memphis provides stream lineage per message with out-of-the-box visualization for each stamped message using a generated header by the Memphis SDK.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## Availability and Messaging

| Parameter                      | Memphis.dev                 | Apache Kafka                |
| ------------------------------ | --------------------------- | --------------------------- |
| Mirroring (Replication)        | Yes                         | Yes                         |
| Multi-tenancy                  | Yes                         | No                          |
| Ordering guarantees            | Consumer group level        | Partition level             |
| Storage tiering                | Yes                         | No. In progress (KIP-405)   |
| Permanent storage              | Yes                         | Yes                         |
| Delivery guarantees            | At least once, Exactly once | At least once, Exactly once |
| Idempotency                    | Yes                         | Yes                         |
| Geo-Replication (Multi-region) | Yes                         | Yes                         |

### Mirroring (Replication)

Kafka Replication means having multiple copies of the data spread across multiple servers/brokers. This helps maintain high availability if one of the brokers goes down and is unavailable to serve the requests.

Memphis station replication works similarly. During station (=topic) creation, the user can choose the number of replicas derived from the number of available brokers. messages will be replicated in a raid-1 manner across the chosen number of brokers.

### Multi-tenancy

Multi-tenancy refers to the mode of operation of software where multiple independent instances of one or multiple applications operate in a shared environment. The instances (tenants) are logically isolated and often physically integrated. The most famous users are SaaS-type applications.

Apache Kafka does not natively support multi-tenancy. It can be achieved via complex client logic, different topics, and ACL.

As Memphis pushes to enable the next generation of applications and especially SaaS-type architectures, Memphis supports Multi-tenancy across all the layers from stations (=topics) to security, consumers, and producers, all the way to node selection for complete hardware isolation in case of need. It is enabled using namespaces and can be managed in a unified console.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-22 at 14.10.43.png" alt=""><figcaption></figcaption></figure>

### Storage tiering

Memphis offers a multi-tier storage strategy in its open-source version. Memphis will write messages that reached their end of 1st retention policy to a 2nd retention policy on object storage like S3 for longer retention time, potentially infinite, and post-streaming analysis. This feature can significantly help with cost reduction and stream auditing.

### Permanent storage

Both Kafka and Memphis store data durably and reliably, much like a normal database. Data retention is user configurable per Memphis station or Kafka topic.

### Idempotency

Both Kafka and Memphis provide default support in idempotent producers.\
On the consumer side, in Kafka, its the client's responsibility to build a retry mechanism that will retransmit a batch of messages exactly once, while in Memphis, it is provided natively within the SDK with a parameter called `maxMsgDeliveries`.

### Geo-Replication (Multi-region)

Common scenarios for a geo-replication include:

* Geo-replication
* Disaster recovery
* Feeding edge clusters into a central, aggregate cluster
* Physical isolation of clusters (such as production vs. testing)
* Cloud migration or hybrid cloud deployments
* Legal and compliance requirements

Kafka users can set up such inter-cluster data flows with Kafka's MirrorMaker (version 2), a tool to replicate data between different Kafka environments in a streaming manner.

Memphis cloud users can create more Memphis clusters and form a supercluster that replicates data in an async manner between the clusters of streamed data, security, consumer groups, unified management, and more.

## Features

| Parameter                   | Memphis.dev                  | Apache Kafka                          |
| --------------------------- | ---------------------------- | ------------------------------------- |
| GUI                         | Native                       | 3rd Party                             |
| Dead-letter Queue           | Yes                          | No                                    |
| Message routing             | Yes                          | Yes. Using Kafka connect and KStreams |
| Log compaction              | Not yet                      | Yes                                   |
| Message replay, time travel | Not yet                      | Yes                                   |
| Stream Enrichment           | SQL and Serverless functions | SQL-based using KStreams              |
| Pull retry mechanism        | Yes                          | Client responsibility                 |

### GUI

Multiple open-source GUIs have been developed for Kafka over the years, for example, [Kafka-UI](https://github.com/provectus/kafka-ui). Usually, it cannot sustain heavy traffic and visualization and requires separate computing and maintenance. There are different commercial versions of Kafka that, among the rest, provide robust GUI, like Confluent, Conduktor, and more.

Memphis provides a native state-of-the-art GUI, hosted inside the broker, built to act as a management layer of all Memphis aspects, including cluster config, resources, data observability, notifications, processing, and more.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Dead-letter Queue

Dead-letter queue is both a concept and a solution that is useful for debugging clients because it lets you isolate and "recycle" instead of drop unconsumed messages to determine why their processing doesn't succeed.

The Kafka architecture does not support DLQ within the broker; it is the client or consumer's responsibility to implement such behavior for good and bad.

One of Memphis' core building blocks is avoiding unexpected data loss, enabling rapid development, and shortening troubleshooting cycles. Therefore, memphis provides a native solution for dead-letter that acts as the station recycle bin for various failures such as unacknowledged messages, schema violations, and custom exceptions.

### Message routing

Kafka provides routing capabilities through Kafka Connect and Kafka Streams, including content-based routing, message transformation, and message enrichment.

Memphis message routing is similar to the implementation of RabbitMQ using routing keys, wildcards, content-based routing, and more. Similar to RabbitMQ, it is also embedded within the broker and does not require external libraries or tools.

### Log compaction

Kafka supports native topic compaction, which runs on all brokers. This runs automatically for compacted topics, condensing the log down to the latest version of messages sharing the same key.

At the&#x20;

### Message replay, time travel

test

### Stream Enrichment

test

### Pull retry mechanism

test

## Conclusion

## Sources

* [https://kafka.apache.org/documentation](https://kafka.apache.org/documentation)
* [https://www.confluent.io](https://www.confluent.io/kafka-vs-pulsar/)
* [https://www.kai-waehner.de/blog](https://www.kai-waehner.de/blog/2022/05/30/error-handling-via-dead-letter-queue-in-apache-kafka/)
