---
description: This section describes the different storage and redundancy options
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Storage and Redundancy



Data redundancy in the field of streaming can be a bit misleading. As written on the [station](station.md) page, in message brokers, data is not preserved for an infinite time but for a defined period based on certain conditions like ingested time, size, and the number of messages within a station.

During the time that data resides in the broker itself, it should be redundant and get removed only when crossing the defined retention policy.



### Storage types

<figure><img src="../../.gitbook/assets/stream file (3).jpeg" alt=""><figcaption></figcaption></figure>

Each station implements a stream object that contains the messages stored in the station. \
It is up to the user to define which type of storage will this stream object be saved.

The options are Memory or Disk.

* **Memory**

<figure><img src="../../.gitbook/assets/storage type memory (1).jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/mem ack.jpeg" alt=""><figcaption><p>Ack process</p></figcaption></figure>

* **Disk**

<figure><img src="../../.gitbook/assets/storage type file.jpeg" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/disk ack.jpeg" alt=""><figcaption><p>Ack process</p></figcaption></figure>

