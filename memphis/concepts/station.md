---
description: This section describes Memphis Station entity
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Station

## What is a station?

A station is a distributed unit that stores messages. Similar to Kafka's topics and RabbitMQ's queues. Each station has a retention policy, which defines when and how messages will be removed from the station—for example, by the number of stored messages, store time, or total size.

Each station is distributed across one or more Memphis brokers, depending on the number of configured station replicas. Data will be poured in a RAID-1 manner.

<figure><img src="../../.gitbook/assets/station.jpeg" alt=""><figcaption><p>How replicas work</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/station_2.jpeg" alt=""><figcaption><p>Produce/Consume data</p></figcaption></figure>

A station is a virtual entity that resides on a type of file called "stream" which stores the data. Stream files are stored on the broker's memory or non-volatile storage, based on the user's configuration per station.&#x20;

### Leaders and followers

the Memphis is based on NATS Jetstream, which makes use of RAFT algorithm as a non-quorum consensus algorithm.

Raft is a consensus algorithm designed as an alternative to the Paxos family of algorithms.\
Raft offers a generic way to distribute a state machine across a cluster of computing systems, ensuring that each node in the cluster agrees upon the same state transitions.

Raft is not a Byzantine fault-tolerant algorithm: the nodes trust the elected leader.

Each station stores a stream component with a single leader on the most available broker for consensus reasons. In case of broker failure, the leader role will be transferred to a follower in configured replicas.

Choosing memory persistency will improve performance, while disk-based persistency will provide higher availability.

<figure><img src="../../.gitbook/assets/stream.jpeg" alt=""><figcaption></figcaption></figure>

### Replicas (Mirroring)

Available in cluster mode only. During station creation, the user can choose the number of station replicas. Replicas are an exact mirror of the entire station data, and each produced message will be mirrored across the configured replicas. Each replica will be stored on a different broker; therefore, the maximum number of replicas is derived from the number of brokers in a cluster.

Replicas can be defined using the SDK, GUI, or CLI.

The number of replicas cannot be changed after station creation. (Will be in the future)

### Retention

Message broker, by design, has a "temporary" nature. Messages can, but not by default deleted when acknowledged to enable new or other consumers from different consumer groups to consume the stored messages.

To avoid filling out the station, we must choose a retention policy per station that defines the condition that will trigger Memphis to remove messages from a station.

* **Number of messages**\
  The station will only retain the last X-produced messages.&#x20;

<figure><img src="../../.gitbook/assets/retention.jpeg" alt=""><figcaption></figcaption></figure>

* **Size**\
  High threshold for station capacity in bytes.
* **Time**\
  Each produced message receives a dedicated timer and will be removed after hitting the configured time.
* **Ack**\
  Messages will be removed from a station once acknowledged by all the connected consumer groups. Great for _exactly-once_ semantics.

### Partitions

A partition is a division of a station or its constituent elements into distinct independent parts. Partitioning is normally done for manageability, performance, and/or availability reasons, and for load balancing.

It is popular in distributed systems, where each partition may be spread over multiple nodes, with users of a station performing local transactions on the partition.

Memphis uses partitions as well. Released on v1.2.

<figure><img src="../../.gitbook/assets/partitions (1).jpeg" alt=""><figcaption></figcaption></figure>

Partitions are the main method of concurrency for stations. \
The used station will be broken into multiple partitions or parts among one or more memphis brokers.

<figure><img src="../../.gitbook/assets/partitions (2).jpeg" alt=""><figcaption></figcaption></figure>

#### How to use partitions

Within each client library manual, you can find where and how to define partitions.

Partitions are defined during station creation, and currently, there is no change needed from the client's side to work with the created partitions.

#### **Limitations**

1. The number of partitions cannot be changed after station creation. It will be available in the future.
2. Currently, producers cannot select which partition to write.
3. Currently, consumers cannot select which partition to read, and it will take place in a round-robin manner.
4. Ordering is guaranteed on the partition level. In case a station is configured with more than one partition, ordering will be transformed into partition level and not the station.

#### Coming up in v2

1. Partition-level produce and consumption.
2. Partition's key assignment. To enable dynamic consumer listening.

### Ordering and delivery

Ordering is guaranteed only while working with a single consumer group.

As seen in the illustration below, each **consumer group** will receive **all** the messages stored within the station.

<figure><img src="../../.gitbook/assets/ordering.jpeg" alt=""><figcaption></figcaption></figure>

## Limitations

| Parameter               | Description                                 | Potential values                       |
| ----------------------- | ------------------------------------------- | -------------------------------------- |
| Max message size        | The maximum message size possible to ingest | Up to 64Mb. By default the size is 8Mb |
| Station name max length | The maximum length of a station name        | Up to 128 characters                   |
|                         |                                             |                                        |

Searched terms: max message, max message size, retention, Retention
