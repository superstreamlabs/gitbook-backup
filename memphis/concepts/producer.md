---
description: This section describes the producer API
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Producer API

## What is a producer?

A producer is the source application/service that pushes data or messages to the broker or more specifically, to the station.&#x20;

As the user configures a client connection to Memphis, it comprises several objects

* Connection - An open socket between the client to Memphis. Only required once as the client/application gets initialized for the first time.
* Producer - A producer entity must be declared to write data/messages into Memphis.
* (And/Or) Consumer - A consumer entity must be declared to read data/messages from Memphis.

<figure><img src="../../.gitbook/assets/Producer.jpeg" alt=""><figcaption></figcaption></figure>

### Broker Data Format

Memphis started from NATS which receives, stores, and sends data in binary format for performance, format alignment, and efficient memory allocations.

When a producer produces messages to Memphis station, they should be converted into binary.

An example from the `node.js` SDK using `Buffer.from` -

```
await producer.produce({
  message: Buffer.from("Message: Hello world"),
});
```

<figure><img src="../../.gitbook/assets/produce 1.jpeg" alt=""><figcaption></figcaption></figure>

### Parameters

(\*) Names might be a bit different from one SDK to another. Meanings are the same.

**Connection**

* `host`: Memphis URL
* `port`: Memphis port
* `username`: Can be root or any other application-type user
* `connectionToken`: The token received when the user created. Will change in the future to more robust credentials and authentication system
* `reconnect`: The connection entity will try to reconnect to Memphis in case of a disconnection
* `maxReconnect`: Amount of time the client will try to reconnect before backing off
* `reconnectIntervalMs`: Time window between one retry to another
* `timeoutMs`: Ability to kill a dead connection after explicit time

**Producer**

* `stationName`: The name of the station to be connected&#x20;
* `producerName`: In a station resolution, each connected producer must have a unique identity

{% hint style="info" %}
For more information about how to connect a producer to Memphis, please head [here](broken-reference)
{% endhint %}

## Scale considerations

A producer is a logical entity that writes data to a Memphis station.\
By adding more producers, the throughput will be increased accordingly due to the additional writers.

## Supported Protocols

* NATS Protocol (Client SDKs)
* HTTP \* Soon \*
* WebSockets \* Soon \*
* gRPC \* Soon \*
* WASM \* Soon \*
* MQTT \* Soon \*

