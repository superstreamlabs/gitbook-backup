---
description: This section describes the Memphis.dev Station
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Station

## What is a station?

The Memphis.dev station functions as a distributed storage unit for messages, resembling the concept of topics in Kafka. Each station is governed by a retention policy, which specifies the criteria for the removal of messages from the station. This policy can be based on factors such as the number of stored messages, storage time, or total size.

Multiple Memphis brokers host each station, the quantity of which depends on the number of configured station replicas. The data distribution within these stations follows a RAID-0 (partitioning) and/or RAID-1 (mirroring) approach, ensuring redundancy and data protection.

<figure><img src="../../.gitbook/assets/station.jpeg" alt=""><figcaption><p>How replicas work</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/station_2.jpeg" alt=""><figcaption><p>Produce/Consume data</p></figcaption></figure>

A station represents a logical entity hosted on a file type known as a "stream," which serves as the storage medium for the data. The storage of stream files can be either in the broker's memory or non-volatile storage, as per the user's configuration for each station.

### Leaders and followers

Memphis is built upon NATS Jetstream, which employs the RAFT algorithm as a non-quorum consensus mechanism. Raft is specifically designed as an alternative to the Paxos family of algorithms.

Raft provides a versatile means to distribute a state machine across a cluster of computing systems, ensuring unanimous agreement on state transitions among every node in the cluster.

It's important to note that Raft is not a Byzantine fault-tolerant algorithm; the nodes rely on the leader elected within the system.

In the Memphis architecture, each station maintains a stream component with a single leader residing on the most readily available broker for the sake of consensus. In the event of a broker failure, the leader role will be seamlessly transferred to a follower within the configured replicas.

Users have the option to choose memory-based persistency, which enhances performance, or opt for disk-based persistency, which provides increased availability.

<figure><img src="../../.gitbook/assets/stream.jpeg" alt=""><figcaption></figcaption></figure>

### Replicas (Mirroring)

This feature is exclusively available in cluster mode. When creating a station, users have the option to specify the number of station replicas. Replicas serve as complete duplicates of all station data, ensuring that every message produced is mirrored across the configured replicas. Each replica is stored on a distinct broker, and the maximum number of replicas is determined by the count of brokers within the cluster.

Replicas can be defined through the SDK, GUI, or CLI interfaces. It's worth noting that, although not currently possible, there are plans to allow changes to the number of replicas after station creation in the future.

### Message Retention

Message brokers are inherently designed with a "transient" characteristic. By default, messages are not automatically deleted upon acknowledgment to allow new consumers or consumers from different groups to access the stored messages.

To prevent overloading the station and ensure efficient message management, it's imperative to select a retention policy for each station. This policy dictates the conditions that will prompt Memphis to remove messages from the station.

* Message Count Limit: The station will retain only the most recent X-produced messages.

<figure><img src="../../.gitbook/assets/retention.jpeg" alt=""><figcaption></figcaption></figure>

* Size: A cap on station capacity, typically defined in bytes, with a high threshold.
* Time: Every produced message is assigned its own timer and will be automatically removed once it reaches the specified configured time limit.
* Ack: Messages within a station will be eliminated when acknowledged by all connected consumer groups, providing robust support for achieving exactly-once semantics.

### Partitions

A partition is a division of a station or its constituent elements into distinct independent parts. Partitioning is normally done for manageability, performance, and/or availability reasons, and for load balancing.

It is popular in distributed systems, where each partition may be spread over multiple nodes, with users of a station performing local transactions on the partition.

Memphis partitions were released on v1.2.

<figure><img src="../../.gitbook/assets/partitions (1).jpeg" alt=""><figcaption></figcaption></figure>

Partitioning is the primary approach to achieving concurrency within stations. A station is divided into multiple partitions, distributed across one or more Memphis brokers.

<figure><img src="../../.gitbook/assets/partitions (2).jpeg" alt=""><figcaption></figcaption></figure>

#### Produce/consume messages when using multiple partitions

Existing types:

1. Round-Robin (Default) - Messages will be produced or consumed to or from a station in a round-robin fashion, meaning each time, a message will be produced or consumed from a different partition. In this way, without a specific logic in place, data will be striped across the created partitions.
2. Partition Key - Producing / Consuming messages with a specified partition key that will allow producers/consumers to work with a single partition. Memphis uses the popular `Murmur3` hash function to convert partition keys into a partition ID.\
   \*Note - Changing the number of partitions will affect the partition key calculation.

Future types:

1. Custom Partition Key Function -\
   Ability to provide a custom callback function to calculate the partition ID from a given partition key.
2. Partition IDs -\
   Specify a partition ID or a subset of IDs for the producer/consumer to work with.\
   If a number of IDs are given, producers/consumers will work in a round-robin fashion with the specified partitions.

If a station has only one partition, messages will be produced/consumed from that partition only. Specifying a partition key or a custom function will be ignored and won’t break the described behavior.

#### How to use partitions

Instructions on how to define and employ partitions can be located within each client library's manual. Partitions are established when creating a station, and presently, no additional action is required on the client's part to interact with the generated partitions.

#### **Limitations**

1. The number of partitions cannot be changed after station creation. It will be available in the future.
2. Currently, producers cannot select which partition to write.
3. Currently, consumers cannot select which partition to read, and it will take place in a round-robin manner.
4. Ordering is guaranteed on the partition level. In case a station is configured with more than one partition, ordering will be transformed into partition level and not the station.

#### Coming up in v2

1. Partition-level produce and consumption.
2. Partition's key assignment. To enable dynamic consumer listening.

### Ordering and delivery guarantee

Memphis.dev provides a strong guarantee of ordering and delivery, ensuring that messages are delivered reliably and in the order in which they were produced. This guarantee is particularly important when working with a single consumer or within a single consumer group, where message order is preserved.&#x20;

In scenarios involving multiple consumer groups, the guarantee of ordering might not hold, as Memphis.dev focuses on delivering high throughput and low latency, potentially sacrificing strict ordering for improved performance. This flexibility allows developers to choose the right trade-off between performance and ordering based on their specific application requirements.



<figure><img src="../../.gitbook/assets/ordering.jpeg" alt=""><figcaption></figcaption></figure>

## Limitations

| Parameter               | Description                                 | Potential values                       |
| ----------------------- | ------------------------------------------- | -------------------------------------- |
| Max message size        | The maximum message size possible to ingest | Up to 64Mb. By default the size is 8Mb |
| Station name max length | The maximum length of a station name        | Up to 128 characters                   |
|                         |                                             |                                        |

Searched terms: max message, max message size, retention, Retention
