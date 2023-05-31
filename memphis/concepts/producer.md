---
description: This section describes Memphis producer API
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

### Broker's Data Format

Memphis forked NATS which receives, stores, and sends data in binary format for performance, format alignment, and efficient memory allocations.

When a producer produces messages to Memphis station, they should be converted into binary.

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

### Parameters

(\*) Names might be a bit different from one SDK to another. Meanings are the same.

**Connection**

* `a host`: Memphis URL
* `port`: Memphis port
* `username`: Can be root or any other application-type user
* `password`: Each application-type user comprises both a username and a password
* `connectionToken`: \*Valid only in case connection-token-based authentication was chosen\*\
  The token received when the user created. Will change in the future to more robust credentials and authentication system
* `reconnect`: The connection entity will try to reconnect to Memphis in case of a disconnection
* `maxReconnect`: Amount of time the client will try to reconnect before backing off
* `reconnectIntervalMs`: Time window between one retry to another
* `timeoutMs`: Ability to kill a dead connection after explicit time
* `keyFile`: In case [encrypted client-Memphis](../../deployment/kubernetes/) communication is used. '\<key-client.pem>'
* `certFile`: In case [encrypted client-Memphis](../../deployment/kubernetes/) communication is used. '\<cert-client.pem>'
* `caFile`: In case [encrypted client-Memphis](../../deployment/kubernetes/) communication is used. '\<rootCA.pem>'

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

* [TCP-based SDKs](broken-reference)
* [HTTP](https://github.com/memphisdev/memphis-http-proxy)
* [WebSockets](https://github.com/orgs/memphisdev/projects/2/views/1?pane=issue\&itemId=14008452) \* Soon \*
* gRPC \* Soon \*
* MQTT \* Soon \*
* AMQP \* Soon \*
* Kafka \* Soon \*

