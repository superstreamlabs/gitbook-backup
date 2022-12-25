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
| License                   | Apache 2.0                               | Apache 2.0              |
| Components                | Memphis + MongoDB (MDB is being removed) | NATS Server + Jetstream |
| Message consumption model | Pull                                     | Pull                    |
| Storage architecture      | Log                                      | Log                     |

### License

Both technologies are available under fully open-source licenses. Memphis also has a Memphis-based distribution with added security, tiered storage, and more.

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



## Comparison Table

| Parameter                          | Memphis.dev                    | NATS Jetstream |
| ---------------------------------- | ------------------------------ | -------------- |
| GUI                                | Yes                            | No             |
| Schema Management                  | Yes                            | No             |
| Stream enrichment                  | Yes                            | No             |
| Produce/Consume behaviour          | Yes                            | No             |
| Consumer group                     | Yes                            | False          |
| Ready-to-use connectors            | Yes                            | Yes            |
| Real-time message tracing          | Yes                            | No             |
| Data-Level Observability           | Yes                            | No             |
| Automatic environment optimization | Yes                            | No             |
| Deduplication                      | Yes. Modified bloom filter     | No             |
| Dead-letter                        | Yes                            | No             |
| REST Gateway                       | Yes                            | No             |
| Consumer internal communication    | Yes. Experimental              | No             |
| Storage tiering                    | Disk, Memory, S3 for Archiving | Disk, Memory   |

