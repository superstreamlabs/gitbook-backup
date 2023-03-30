---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Message broker

### A message broker is&#x20;

A software that implements an architectural pattern for message transportation, transformation, and routing. It mediates communication among applications, minimizing the mutual awareness that applications should have of each other in order to be able to exchange messages, effectively implementing decoupling.

### Purpose&#x20;

The primary purpose of a broker is to take incoming messages from applications and perform some action on them. Message brokers can decouple end-points, meet specific non-functional requirements, and facilitate the reuse of intermediary functions. For example, a message broker may be used to manage a workload queue or message queue for multiple receivers, providing reliable storage, guaranteed message delivery, and transaction management.

How applications communicate is becoming an increasingly large challenge. Using messaging middleware simplifies this challenge and allows for a common communications infrastructure that grows and scales to meet the most demanding conditions

### Differences between Broker, Pub/Sub, Queue

A message broker is a software component that passes messages between two systems. A queue is a data structure that can store messages and pass them to the application when they are ready. A pub/sub system is used to publish and subscribe to events.

**Message Broker (Memphis)**

A message broker is an intermediary between two applications or services. It accepts requests from one application or service, processes them and returns responses back to the requester. The main purpose of a message broker is to decouple services from each other by handling communication between them.

**Queues**

A queue is a data structure used for storing messages/events in order of arrival until they can be processed by an application/service. Queues are useful for decoupling components in distributed systems because they allow asynchronous processing of messages through multiple nodes in the system — hence allowing us to scale our applications easily.

**Publish-Subscribe System**

A publish-subscribe system (also known as Pub-Sub) allows you to automatically route information throughout your application based on rules defined by publishers and subscribers (instead of having direct connections between publishers and subscribers).

### Memphis' most popular use cases&#x20;

* Real-time streaming pipelines
* Async task management
* Data ingestion
* Async communication between services on k8s
* Queuing
* Multiple destinations to a single message

## Supported Protocols

* [TCP-based SDKs](broken-reference)
* [HTTP](https://github.com/memphisdev/memphis-http-proxy)
* WebSockets \* Soon \*
* gRPC \* Soon \*
* WASM \* Soon \*
* MQTT \* Soon \*
