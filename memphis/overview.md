# Overview

### What is Memphis{dev}?

Memphis{dev} **is** the only low-code messaging platform that provides a full ecosystem **for** in-app streaming use cases over a distributed message broker with a paradigm of produce-consume **that** supports modern in-app streaming pipelines and async communication **by** removing frictions of management, cost, resources, language barriers, and time **for** data-oriented developers and data engineers, **unlike** other message brokers and queues that requires a great amount of code, optimizations, adjustments, and mainly time.

[memphis.dev](https://memphis.dev/) started as a fork of the [nats.io](http://nats.io/) project (since 2011), written in GoLang, and creating its own stream on top. Instead of topics and queues, Memphis uses stations, which will become an entity with embedded logic in the future.

### What struggles does Memphis solve?

1. Too many data sources become too complicated to handle.
2. Different and complex schemas get in the way.
3. It's difficult to transform schema and analyze streamed data per source.
4. Currently, stream processing requires stitching multiple applications together like Apache Kafka, Flink, and NiFi. Real-time becomes near-real-time, or far-real-time.
5. Loss of messages due to lack of retransmits, crashes, and monitoring.
6. The event's journey is challenging to debug and troubleshoot.
7. Kafka, RabbitMQ, NATS, and other MQs are HARD to deploy, manage, secure, update, onboard, and tune.
8. Turning batch processes into real-time can be complicated and time-consuming.

### Features

**Current**

* Fully optimized message broker in under 3 minutes
* Easy-to-use UI, CLI, and SDKs
* Data-level observability
* Runs on your Docker or Kubernetes
* Message Journey
* SDKs: Node.js, Typescript, Python, Go

**Coming**

* Embedded schema management using Avro, and Protobuf
* Ready-to-use connectors and analysis functions
* More SDKs
* Inline processing
