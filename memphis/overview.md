---
description: v1.0.0 - LTS
cover: ../.gitbook/assets/Overview..jpg
coverY: 0
---

# Overview

## What is Memphis{dev}?

**Memphis** is a next-generation alternative to traditional message brokers.

A simple, robust, and durable cloud-native message broker wrapped with an entire ecosystem that enables cost-effective, fast, and reliable development of modern queue-based use cases.

Memphis enables the building of modern queue-based applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.

### **Memphis focuses on four pillars.**

1. Stability - Queues and brokers play a critical part in the modern application's structure and should be highly available and stable as possible.
2. Performance and Efficiency - Provide good performance while maintaining efficient resource consumption.
3. Observability - Reduces troubleshooting time to near zero.
4. Developer Experience - Rapid Development, Modularity, inline processing, Schema management.

## A world without Memphis

When your application requires a message broker or a queue,\
Implementing one will require you to -

* Build a dead-letter queue, create observability, and a retry mechanism
* Build a scalable environment
* Create client wrappers
* Tag events to achieve multi-tenancy
* Enforce schemas and handle transformations
* Handle back pressure. Client or queue side
* Configure monitoring and real-time alerts
* Create a cloud-agnostic implementation
* Create config alignment between production to a dev environment
* Spent weeks and months learning the internals through archival documentation, ebooks, and courses
* Onboard your developers

And the list continues...

### The use cases behind the struggles.

* Too many data sources become too complicated to handle.
* Different and complex schemas get in the way.
* It isn't easy to transform schema and analyze streamed data per source.
* Currently, stream processing requires stitching multiple applications together, like Apache Kafka, Flink, and NiFi. Real-time becomes near-real-time or far-real-time.
* Loss of messages due to lack of retransmits, crashes, and monitoring.
* The event's journey is challenging to debug and troubleshoot.
* Kafka, RabbitMQ, NATS, and other MQs are HARD to deploy, manage, secure, update, onboard, and tune.
* Turning batch processes into real-time can be complicated and time-consuming.

## Key Features (v0.1)

* Fully optimized message broker in under 3 minutes
* Easy-to-use UI, CLI, and SDKs
* Dead-letter station (DLQ)
* Data-level observability
* [Storage tiering](broken-reference), including remote storage classes like AWS S3
* Runs on your Docker or Kubernetes
* Real-time event tracing
* SDKs: Python, Go, Node.js, Typescript, Nest.JS, Kotlin, .NET, Java, REST
* Embedded schema management using Protobuf, JSON Schema, GraphQL, Avro
* [Integration center](broken-reference)

A full roadmap can be found here [https://memphis.dev/roadmap](https://memphis.dev/roadmap)

## **Memphis core team**

We run as one, from marketing to operations to product to development, growing fast and constantly evolving, learning, and doing our best to enjoy the ride.

We highly encourage learning, writing, meetups, contributions to other OSS projects, and community activities.

Our culture is to lead every team member individually to achieve great personal development while maintaining a healthy work-life balance.

As an open-source first company, we consider our precious contributors an essential part of our team, and we do our best to cherish every contributor.

## **Vision**

Our mission is to build the best-in-class dev-first, open-source event-processing platform that will enable millions of developers to better engage with data and build greater data-driven applications and streaming pipelines in friction of time and complexity.

## **Open-Source**

Memphis is an open-source first product and company. Memphis will always be open-source and community-driven as we consider the community as the source of power behind building truly disruptive technology.

### <img src="../.gitbook/assets/image (3) (2) (1).png" alt="" data-size="line">

## **Team Locations**

Delaware, US

Tel-Aviv, Israel

Scotland, UK

## License

Memphis is open-sourced and operates under the "Memphis Business Source License 1.0" license.

Built out of Apache 2.0, the main difference between the licenses is:

Additional Use Grant: You may make use of the Licensed Work (i) only as part of your own product or service, provided it is not a message broker or a message queue product or service; (ii) provided that you do not use, provide, distribute, or make available the Licensed Work as a Service;

A “Service” is a commercial offering, product, hosted, or managed service, that allows third parties (other than your own employees and contractors acting on your behalf) to access and/or use the Licensed Work or a substantial set of the features or functionality of the Licensed Work to third parties as a software-as-a-service, platform-as-a-service, infrastructure-as-a-service or other similar services that compete with Licensor products or services.
