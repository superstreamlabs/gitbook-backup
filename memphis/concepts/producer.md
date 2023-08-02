---
description: This section describes Memphis producer API
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Producer API

## What is a producer?

A producer is the source application/service that pushes data or messages to the broker.

When a client connection to Memphis gets created, it comprises multiple objects:

* Connection - An open socket between the client to Memphis.
* Producer - A producer entity must be declared to write data/messages into a Memphis station.
* (And/Or) Consumer - A consumer entity must be declared to read data/messages from a Memphis station.

<figure><img src="../../.gitbook/assets/Producer.jpeg" alt=""><figcaption></figcaption></figure>

### Broker's Data Format

Memphis reads, stores, and writes data in a binary format for performance, format alignment, and efficient memory allocations. When a producer produces a message to a Memphis station, it will be converted into binary.

An example from the `node.js` SDK using `Buffer.from` -

```
await producer.produce({
  message: Buffer.from("Message: Hello world"),
});
```

<figure><img src="../../.gitbook/assets/produce 1.jpeg" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
**Unexist stations** will be created **automatically** through the SDK on the first producer/consumer connection.
{% endhint %}

{% hint style="info" %}
Full API guide and code examples can be found here [Broken link](broken-reference "mention")
{% endhint %}

## Performance Optimizations

### 1. Non-blocking producer

By choosing a non-blocking type of producer, the client will not block the next system call or message producing until an acknowledgment has been received by the client from the broker requesting to send the message.&#x20;

Using that option enables the client's CPU to utilize its resources in greater parallelism and avoid idle waiting.

The parameter is called `asyncProduce` and can be found in any supported client library [here](broken-reference) or through the following example -&#x20;

```java
await producer.produce({
    message: 'test'
    asyncProduce: true // true for non-blocking, false for blocking
});
```

### 2. Partitions

Memphis uses partitions.

Partitions are the main method of concurrency for stations. \
The used station will be broken into multiple partitions or parts among one or more memphis brokers.

Memphis' current partitions architecture has implemented a 1:1 station-to-partition ratio, meaning that in the current version of Memphis (1.0.2), to be able to benefit from the partitions' concurrency ability, the user needs to consider a station as a single partition, meaning if more concurrency is needed, than you should add use more stations as the number of partitions needed.

This will be fixed by 1.1.

<figure><img src="../../.gitbook/assets/partitions 1.jpeg" alt=""><figcaption><p>Future partitions</p></figcaption></figure>

## Supported Protocols

* [TCP-based SDKs](broken-reference)
* [HTTP](https://github.com/memphisdev/memphis-http-proxy)

