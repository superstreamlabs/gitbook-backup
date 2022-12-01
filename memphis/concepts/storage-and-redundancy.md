---
description: This section describes the different storage and redundancy options
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Storage and Redundancy

## Introduction

Data redundancy in the field of streaming can be a bit misleading. As written on the [station](station.md) page, in message brokers, data is not preserved for an infinite time but for a defined period based on certain conditions like ingested time, size, and the number of messages within a station.

When data resides in the broker, it will be redundant and removed only when crossing the defined retention policy.

### The object behind the station - Stream

Each station implements a stream object that contains the messages stored in the station. \
It is up to the user to define which type of storage will this stream object be saved.

<figure><img src="../../.gitbook/assets/stream file (3).jpeg" alt=""><figcaption></figcaption></figure>

## Storage tiering

Memphis offers a range of storage types that you can choose from based on your workload's data access, resiliency, frequency, and cost requirements, and configured per station.

### Tier 1 (Hot storage)

The first type of storage each message will initially be stored at.

The options are Memory or Disk. Each with its strengths and weaknesses.

* **Memory.**\
  ****For faster performance.\
  Due to its nature as a volatile type of storage, the risk of losing data in case of failure is higher because it resides in the broker's memory, and in the case of a station without configured replicas, data can be lost.

<figure><img src="../../.gitbook/assets/storage type memory (1).jpeg" alt=""><figcaption><p>Stream object as it construct and stored</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/mem ack.jpeg" alt=""><figcaption><p>Ack process</p></figcaption></figure>

* **Disk.**\
  ****For higher availability.\
  Disk storage might be slower than memory, but it offers greater availability and resiliency to broker failures.

<figure><img src="../../.gitbook/assets/storage type file.jpeg" alt=""><figcaption><p>Stream object as it construct and stored on disk storage</p></figcaption></figure>



<figure><img src="../../.gitbook/assets/disk ack.jpeg" alt=""><figcaption><p>Ack process</p></figcaption></figure>

### Tier 2 (Cold storage)
