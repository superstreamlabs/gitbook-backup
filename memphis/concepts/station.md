---
description: Page under construction
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Station

### What is a station?

A station is a distributed unit that stores messages. Similar to Kafka's topics and RabbitMQ's queues. Each station has a retention policy, which defines when and how messages will be removed from the station—for example, by the number of stored messages, store time, or total size of stored messages.

Each station is distributed across one or more Memphis brokers, depending on the number of configured station replicas.

<figure><img src="../../.gitbook/assets/station.jpeg" alt=""><figcaption></figcaption></figure>

![](<../../.gitbook/assets/station\_2 (1).jpeg>)

### Station store types

* Memory
* File

### Retention

* Number of messages
* Station size
* Time

### High Availability

#### Replicas





