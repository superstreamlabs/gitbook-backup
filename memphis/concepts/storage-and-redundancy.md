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

Each station implements a stream object that contains the messages stored in the station.\
It is up to the user to define which type of storage will this stream object be saved.

<figure><img src="../../.gitbook/assets/stream.jpeg" alt=""><figcaption></figcaption></figure>

## Replicas (Mirroring)

Available in cluster mode only.

During station creation, the user can choose the number of station replicas. Replicas are an exact mirror of the entire station data, and each produced message will be mirrored across the configured replicas. Each replica will be stored on a different broker; therefore, the maximum number of replicas is derived from the number of brokers in a cluster.\
In case of a broker or disk loss, replicas will be used to rebuild the missing replica to maintain the required amount of replicas and, at the same time, ensure data availability through a different broker.

Replicas can be defined using the SDK, GUI, or CLI.

The number of replicas cannot be changed after station creation (but can be in the future)

## Storage tiering

Memphis offers a range of storage types that you can choose from based on your workload's data access, resiliency, frequency, and cost requirements, and configured per station.

### Tier 1 (Local storage)

The first type of storage each message will initially be stored at.

The options are Memory or Disk. Each with its strengths and weaknesses.

* **Memory.**\
  For faster performance.\
  Due to its nature as a volatile type of storage, the risk of losing data in case of failure is higher because it resides in the broker's memory, and in the case of a station without configured replicas, data can be lost.

<figure><img src="../../.gitbook/assets/storage type memory.jpeg" alt=""><figcaption><p>Stream object as it construct and stored</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/mem ack.jpeg" alt=""><figcaption><p>Ack process</p></figcaption></figure>

* **Disk.**\
  For higher availability.\
  Disk storage might be slower than memory, but it offers greater availability and resiliency to broker failures.

<figure><img src="../../.gitbook/assets/disk.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/disk ack.jpeg" alt=""><figcaption><p>Ack process</p></figcaption></figure>

### Tier 2 (Remote storage) \* Optional \*

The common pattern of message brokers is to delete messages after passing the defined retention policy, like time/size/number of messages.\
Memphis offers a 2nd storage tier for longer, possibly infinite retention for stored messages.\
Each message that expels from the station will automatically migrate to the 2nd storage tier.\
Possible integrations [here](../../integrations/storage/).

#### Behind the scenes

<figure><img src="../../.gitbook/assets/storage tier arch (1).jpeg" alt=""><figcaption></figcaption></figure>

#### A growing list of options:

* [**S3 (Object storage)**](../../integrations/storage/amazon-s3.md)\
  Built to store and retrieve any amount of data from anywhere using S3 protocol.\
  Object storage offers different storage classes with different costs and performance requirements.\
  Popular S3-based storage providers are: AWS S3, MinIO, IBM Cloud Object Storage, and more.
* **MinIO \*soon\***
* **Azure blob storage \*soon\***
* **GCP cloud storage \*soon\***
