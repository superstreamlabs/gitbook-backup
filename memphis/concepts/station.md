---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Station

### What is a station?

A station is a distributed unit that stores messages. Similar to Kafka's topics and RabbitMQ's queues. Each station has a retention policy, which defines when and how messages will be removed from the station—for example, by the number of stored messages, store time, or total size of stored messages.

Each station is distributed across one or more Memphis brokers, depending on the number of configured station replicas. Data will be poured in a RAID-1 manner.

<figure><img src="../../.gitbook/assets/station.jpeg" alt=""><figcaption><p>How replicas work</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/station_2.jpeg" alt=""><figcaption><p>Produce/Consume data</p></figcaption></figure>

A station is a virtual entity that resides on a type of file called "stream" which actually stores the data itself. Stream files are stored on the broker's memory or non-volatile storage, based on the user's configuration per station.&#x20;

Each station stores a stream component with a single leader on the most available broker for consensus reasons. In case of broker failure, the leader role will be transferred to a different broker.

Naturally, choosing memory persistency will improve performance, while file-based persistency will provide higher resiliency.

<figure><img src="../../.gitbook/assets/stream file.jpeg" alt=""><figcaption></figcaption></figure>

Each stream contains the following objects

* meta.inf: metadata of the station
* meta.sum: hash calculation of the station's metadata
* msgs: The stored messages

### Station storage types

* **Memory** - Improved performance

<figure><img src="../../.gitbook/assets/storage type memory.jpeg" alt=""><figcaption></figcaption></figure>

* **Disk** - Improved resiliency

<figure><img src="../../.gitbook/assets/storage type file (1).jpeg" alt=""><figcaption></figcaption></figure>

### Retention

In a message broker, messages are not deleted when acknowledged to enable new or other consumers from different consumer groups to consume the stored messages as well.

To avoid filling out the station, we must choose a retention policy per station that defines the condition that will trigger Memphis to remove messages from a station.

* Number of messages.

The station will only retain the last X produced messages.&#x20;

<figure><img src="../../.gitbook/assets/retention.jpeg" alt=""><figcaption></figcaption></figure>

* Station size

High threshold for station capacity in bytes.

* Time

Each produced message receives a dedicated timer and will be removed after hitting the configured time.

Searched terms: retention, Retention



