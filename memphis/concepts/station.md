---
description: Page under construction
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Station

### What is a station?

In Memphis, a station is the unit that holds messages. It's like a topic in Kafka or a queue in RabbitMQ. Each station has a retention policy, which defines when or how messages will be removed from the station—for example, by number of messages, time, or total size of stored messages.

Each station is spread across one or more Memphis nodes, depending on the number of configured replicas.

Each replica is an exact duplicate of the entire data set, stored on a different node for high availability (HA).

\*Architecture\*

### Message store types

* Memory
* File

### Retention

* Number of messages
* Station size
* Time

### High Availability



