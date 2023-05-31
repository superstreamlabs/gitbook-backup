---
description: This section describes the differences between NATS Jetstream and Memphis
cover: ../../.gitbook/assets/Jetstream vs Memphis.jpeg
coverY: 0
---

# NATS Jetstream vs Memphis

## **What is NATS?**

​​NATS is an open-source, message bus technology that powers modern distributed systems. Accordingly, it is responsible for addressing, discovering, and exchanging messages that drive the common patterns in distributed systems, asking and answering questions, aka services/microservices, and making and processing statements, or stream processing.

### Streaming using NATS Jetstream

NATS has a built-in distributed persistence system called Jetstream. Specifically, this system enables new functionalities and higher qualities of service on top of the base 'Core NATS' functionalities and qualities of service.\
Jetstream is built-in to nats-server; therefore, you only need 1 (or 3 or 5 if you want fault-tolerance against 1 or 2 simultaneous NATS server failures) of your NATS server(s) to be JetStream enabled for it to be available to all the client applications.

## **What is Memphis.dev?**

After years of operating and researching NATS, Memphis refactored NATS Jetstream to create a fully-fledged event-processing platform for developers.

Memphis.dev enables building next-generation applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.

### **Memphis focuses on four pillars**

1. Stability - Queues and brokers play a critical part in the modern application's structure and should be highly available and stable as possible.
2. Performance and Efficiency - Provide good performance while maintaining efficient resource consumption.
3. Observability - Reduces troubleshooting time to near zero.
4. Developer Experience - Rapid Development, Modularity, inline processing, Schema management.

## General

| Parameter                 | Memphis.dev                              | NATS Jetstream          |
| ------------------------- | ---------------------------------------- | ----------------------- |
| License                   | BSL 1.0                                  | Apache 2.0              |
| Components                | Memphis + MongoDB (MDB is being removed) | NATS Server + Jetstream |
| Message consumption model | Pull                                     | Pull                    |
| Storage architecture      | Log                                      | Log                     |

### License

Both technologies are available under fully open-source licenses. Memphis also has a commercial-based distribution with added security, tiered storage, and more.

### Components

Memphis uses MongoDB for GUI state management only and will be removed soon, making Memphis without any external dependency. Both Memphis and Jetstream achieve consensus by using RAFT.

### Message Consumption Model

Jetstream and Memphis use a pull-based architecture where consumers pull messages from the server, and [long-polling](https://en.wikipedia.org/wiki/Push\_technology#Long\_polling) is used to ensure new messages are made available instantaneously.

Pull-based architectures are often preferable for high throughput workloads as they allow consumers to manage their flow control, fetching only what they need.

### Storage Architecture

Jetstream and Memphis use a distributed commit log called streams (made by NATS Jetstream) as its storage layer, which can be written entirely on the broker's (server) memory or disk.\
Memphis abstracts the offsets by default, so saving a record of the used offsets resides on Memphis and not on the client.\
Memphis also offers storage tiering for offloading messages to S3-compatible storage for infinite and more cost-effective storage. Reads are sequential.

## Ecosystem and User Experience

| Parameter                            | Memphis.dev             | NATS Jetstream                  |
| ------------------------------------ | ----------------------- | ------------------------------- |
| Deployment                           | Helm, Docker, Terraform | Binary, Helm, Docker, Terraform |
| Enterprise support                   | Yes                     | Synadia                         |
| Managed cloud offerings              | Yes                     | Synadia                         |
| Self-Healing                         | Yes                     | No                              |
| Notifications                        | Yes                     | Partial                         |
| Message tracing (aka Stream lineage) | Yes                     | No                              |

### Deployment

NATS Jetstream and Memphis have a lightweight yet robust cloud-native architecture. Memphis and Jetstream are packed as a container from day one. It can be deployed using any docker engine, docker swarm, and for production environment using helm for Kubernetes (soon with operator). While Jetstream requires a config file and different parameters, Memphis' initial config is already sufficient for production, and modifications and optimizations can take place on-the-fly without downtime. While the end outcome can be achieved in both technology, Memphis abstracts most of the underlying that in Jetstream, requires manual configuration in Kubernetes.

### Enterprise support and managed cloud offering

Enterprise-grade support and managed cloud offerings for NATS are available from Synadia.

Memphis provides enterprise support and managed cloud offering that includes features like enhanced security, stream research abilities, an ML-based resource scheduler for better cost savings, and more.

### Self-healing

When deploying NATS Jetstream over Kubernetes, the natural life-cycle management of Kubernetes is received by default like automatic broker restoration, PVC claim, data restoration, and more.

In addition to the above, one of Memphis' core features is to remove frictions of management and autonomously make sure it's alive and performing well using periodic self-checks and proactive rebalancing tasks, as well as fencing the users from misusing the system. In parallel, every aspect of the system can be configured on-the-fly without downtime, including upgrades and reboots.

### Notifications

The [NATS Surveyor](https://github.com/nats-io/nats-surveyor) system has initial support for passing JetStream metrics to Prometheus, dashboards and more will be added toward the final release.

Memphis has a built-in notification center that can push real-time alerts based on defined triggers like client disconnections, resource depletion, schema violation, and more.

<figure><img src="../../.gitbook/assets/image (3) (5).png" alt=""><figcaption></figcaption></figure>

### Message tracing (aka Stream lineage)

Tracking stream lineage is the ability to understand the full path of a message from the very first producer through the final consumer, including the trail and evolvement of a message between topics. This ability is extremely handy in a troubleshooting process.

NATS Jetstream does not provide a native ability for stream lineage, but it can be achieved using OpenTelemetry or OpenLineage frameworks, as well as integrating 3rd party applications such as datadog, epsagon, and more.

Memphis provides stream lineage per message with out-of-the-box visualization for each stamped message using a generated header by the Memphis SDK.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## Availability and Messaging

| Parameter                      | Memphis.dev                 | NATS Jetstream              |
| ------------------------------ | --------------------------- | --------------------------- |
| Mirroring (Replication)        | Yes                         | Yes                         |
| Multi-tenancy                  | Yes                         | Yes                         |
| Ordering guarantees            | Consumer group level        | Consumer level              |
| Storage tiering                | Yes                         | No                          |
| Permanent storage              | Yes                         | Yes                         |
| Delivery guarantees            | At least once, Exactly once | At least once, Exactly once |
| Idempotency                    | Yes                         | Yes                         |
| Geo-Replication (Multi-region) | Yes                         | Yes                         |

### Mirroring (Replication)

NATS Queues and Memphis station replication work similarly. During station (=queue) creation, the user can choose the number of replicas derived from the number of available brokers. messages will be replicated in a raid-1 manner across the chosen number of brokers.

### Multi-tenancy

Multi-tenancy refers to the mode of operation of software where multiple independent instances of one or multiple applications operate in a shared environment. The instances (tenants) are logically isolated and often physically integrated. The most famous users are SaaS-type applications.

JetStream is compatible with NATS 2.0 Multi-Tenancy using Accounts. A JetStream-enabled server supports creating fully isolated JetStream environments for different accounts.

As Memphis pushes to enable the next generation of applications and especially SaaS-type architectures, Memphis supports Multi-tenancy across all the layers from stations (=queues) to security, consumers, and producers, all the way to node selection for complete hardware isolation in case of need. It is enabled using namespaces and can be managed in a unified console.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

### Storage tiering

Memphis offers a multi-tier storage strategy in its open-source version. Memphis will write messages that reached their end of 1st retention policy to a 2nd retention policy on object storage like S3 for longer retention time, potentially infinite, and post-streaming analysis. This feature can significantly help with cost reduction and stream auditing.

### Permanent storage

Both Jetstream and Memphis store data durably and reliably, much like a normal database. Data retention is user configurable per Memphis station or NATS queue.

### Idempotency

Both Jetstream and Memphis provide default support in idempotent producers.\
On the consumer side, in Jetstream, its the client's responsibility to build a retry mechanism that will retransmit a batch of messages exactly once, while in Memphis, it is provided natively within the SDK with a parameter called `maxMsgDeliveries`.

### Geo-Replication (Multi-region)

Common scenarios for a geo-replication include:

* Geo-replication
* Disaster recovery
* Feeding edge clusters into a central, aggregate cluster
* Physical isolation of clusters (such as production vs. testing)
* Cloud migration or hybrid cloud deployments
* Legal and compliance requirements

Jetstream uses a concept called gateways to establish a "super cluster."\
Gateways enable connecting one or more clusters into a full mesh;

Memphis cloud users can create more standard Memphis clusters and form a super cluster that replicates streamed data in an async manner between the clusters, including mirroring security, unified management, and more. At the moment, in such a configuration, only one cluster is considered to be writable, and the rest are read replicas meaning they are only available for message consumption.

## Features

| Parameter                            | Memphis.dev                          | NATS Jetstream          |
| ------------------------------------ | ------------------------------------ | ----------------------- |
| GUI                                  | Yes                                  | No                      |
| Schema Management                    | Yes                                  | No                      |
| Stream enrichment                    | Yes                                  | No                      |
| Delivery Policy                      | Manual Ack                           | Different options       |
| Consumer group                       | Yes                                  | Yes                     |
| Ready-to-use source/sinks connectors | Yes                                  | No                      |
| Stream lineage                       | Yes                                  | No                      |
| Data-Level Observability             | Yes                                  | No                      |
| Wildcard subscribe                   | No                                   | Yes                     |
| Deduplication                        | Content-level. Modified bloom filter | Message ID-level        |
| Dead-letter queue                    | Yes                                  | Notification event only |
| REST Gateway                         | Yes                                  | No                      |
| Consumer internal communication      | Yes. Experimental                    | No                      |
| Pull retry mechanism                 | Yes                                  | Yes                     |
| Message replay (time travel)         | Yes. Offsets                         | Yes. Offsets            |
| Content-aware routing                | Yes                                  | No                      |
| Log compaction                       | Not yet                              | No                      |

### GUI

A GUI for NATS and NATS Jetstream can be achieved via Synadia cloud offering.

Memphis provides a native state-of-the-art GUI, hosted inside the broker, built to act as a management layer of all Memphis aspects, including cluster config, resources, data observability, notifications, processing, and more.

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt=""><figcaption><p>Memphis GUI</p></figcaption></figure>

### Schema Management

The very basic building block to control and ensure the quality of data that flows through your organization between the different owners is by defining well-written schemas and data models.

NATS or NATS Jetstream does not offer schema management.

As part of its open-source version, Memphis presents Schemaverse, which is also embedded within the broker. Schemaverse provides a robust schema store and schema management layer on top of memphis broker without a standalone compute or dedicated resources. With a unique and modern UI and programmatic approach, technical and non-technical users can create and define different schemas, attach the schema to multiple stations and choose if the schema should be enforced or not. In counter to Schema Registry, the client does not need to implement serialization functions, and every schema update takes place during producers' runtime.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-24 at 22.32.36.png" alt=""><figcaption><p>Memphis Schemaverse</p></figcaption></figure>

### Stream Enrichment

NATS Jetstream does not offer stream enrichment.

Memphis provides a similar behavior and more. Embedded inside the broker, Memphis users can create serverless-type functions or complete containerized applications that aggregate several stations and streams, decorate and enrich messages from different sources, write complex functions that cannot be achieved via SQL, and manipulate the schema. Memphis embedded connectors frameworks will help to push the results directly to a defined sink.

### Delivery Policy

The way message delivery will take place with new consumers to be able to control the consumption once a new consumer subscribes to a queue or station.

Jetstream supports different options, such as DeliverAll / DeliverLast / DeliverLastPerSubject / and more.

Memphis currently supports only manual acks.

### Ready-to-use source/sinks connectors

NATS offers a rich ecosystem of integrations between different systems with NATS.

Memphis offers both integrations similar to NATS due to its compatibility with NATS protocol, Memphis-based integrations, and ready-to-use connectors that can replace producers and consumers to and from different applications and databases.

### Stream lineage

Tracking stream lineage is the ability to understand the full path of a message from the very first producer through the final consumer, including the trail and evolvement of a message between topics. This ability is extremely handy in a troubleshooting process.

NATS Jetstream does not offer stream lineage, but it can be achieved using OpenTelemetry or OpenLineage frameworks or integrating 3rd party applications such as Datadog and Epsagon.

Memphis provides stream lineage per message with out-of-the-box visualization for each stamped message using a generated header by the Memphis SDK.

<figure><img src="../../.gitbook/assets/image (5) (3) (1).png" alt=""><figcaption></figcaption></figure>

### Data-Level Observability

The ability to view ingested messages' payload and metadata.

Memphis offers that ability within the GUI.

![](<../../.gitbook/assets/Screen Shot 2022-12-27 at 23.53.03.png>)

### Dead-letter queue

Dead-letter queue is both a concept and a solution that is useful for debugging clients because it lets you isolate and "recycle" instead of drop unconsumed messages to determine why their processing doesn't succeed.

In NATS Jetstream, whenever a message reaches its maximum number of delivery attempts, an advisory message is published on the `$JS.EVENT.ADVISORY.CONSUMER.MAX_DELIVERIES.<STREAM>.<CONSUMER>` subject. The advisory message's payload (use `nats schema info io.nats.jetstream.advisory.v1.max_deliver` for specific information) contains a `stream_seq` field that contains the sequence number of the message in the stream.

One of Memphis' core building blocks is avoiding unexpected data loss, enabling rapid development, and shortening troubleshooting cycles. Therefore, memphis provides a native solution for a dead-letter queue that acts as the station recycle bin for various failures such as unacknowledged messages, schema violations, and custom exceptions. the DLQ also supports in resend messages directly to the consumer who didn't ack the message or back to the station for reuse.

![](<../../.gitbook/assets/Screen Shot 2022-12-27 at 23.58.52.png>)

### REST Gateway

To enable message production via HTTP calls for various use cases and ease of use, Memphis added an HTTP gateway to receive REST-based requests (=messages) and produce those messages to the required station.

### Consumer internal communication

In some use cases, especially around microservices and streaming pipelines, it is crucial for the different microservices to be able to communicate with each other to sync changes, arrival of data, match data flow, etc.

Memphis is currently experimenting with creating that ability for consumers to be able to communicate with each other using gRPC or shared channels.

### Pull retry mechanism

Jetstream and Memphis provide a message retry mechanism based on the "max redelivery" parameter.

In many other technologies, this is the client's responsibility.

### Message replay (time travel)

The ability to replay acknowledge messages based on a specific offset.

Currently not supported in Memphis or Jetstream.

### Content-aware routing

This feature, in other words, can be called an "Event Router." It enables routing messages to different consumers based on payload and predefined rules.

Memphis is currently experimenting with that [feature](https://github.com/memphisdev/memphis-broker/issues/283). Jetstream currently does not support it.

### Log compaction

Compaction has been created to support a long-term, potentially infinite record store based on specific keys.

At the moment, Memphis and Jetstream do not support compaction.

## Conclusion

NATS Jetstream is a battle-tested distributed messaging system, fairly modular, and easy to implement. Those features, among others, motivated Memphis to choose NATS as the foundation. Having said it, extending NATS to high throughput workloads can be complicated and often unstable (not necessarily because of NATS, but missing implementation from the client side). Like other technologies, it requires time and resources you can save.

We call Memphis "A next-generation message broker" because it leans towards the user and adapts to its scale and requirements, not the opposite. Most of the wrappers, tunings, management-overhead, and implementations needed from the client in Jetstream, are abstract to the users in Memphis, which provides an excellent solution for both the smaller workload use cases and the more robust ones under the same system and with full ecosystem to support it. It has a milage to pass, but the immediate benefits already exist and will continue to evolve.
