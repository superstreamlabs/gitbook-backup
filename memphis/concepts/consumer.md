---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Consumer API

### What is a consumer?

A consumer is a client that reads data or messages from the broker or more specifically from the station.&#x20;

As the user configures a client connection to Memphis, it comprises several objects

* Connection - An open socket between the client to Memphis. Only required once as the client/application gets initialized for the first time.
* Consumer - A consumer entity must be declared to read data/messages from Memphis.
* (And/Or) Producer - A producer entity must be declared to write data/messages into Memphis.

<figure><img src="../../.gitbook/assets/Producer.jpeg" alt=""><figcaption></figcaption></figure>

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

**Consumer**

* `stationName`: The name of the station to be connected
* `consumerName`: In a station resolution, each connected consumer must have a unique identity
* `consumerGroup`: Explained in detail [here](consumer-groups.md). Consumers are grouped under an object called "Consumer group." If not specified, a default CG will be created using the _consumerName_
* `pullIntervalMs`: Configured in milliseconds, this parameter defines the intervals of each consume operation. For example, if the value is set to 1000, it means that every 1000 ms, the consumer will try to pull new messages
* `batchSize`: Defines how many messages will be collected per pull operation
* `batchMaxTimeToWaitMs`: Defines how much time (in milliseconds) the consumer should wait for the entire required batch to be collected
* `maxAckTimeMs`: For the consumer to receive the next message, the current one must be acknowledged, meaning the consumer is ready to consume and handle the next message. Oftentimes, the consumer gets crashed/throws an exception / not able to handle the message. The _`maxAckTimeMs` _ ensure that until X millisecond Memphis has not received ACK, it will automatically retransmit the message. If not configured correctly, it can result in a duplicate processing
* `maxMsgDeliveries`: The number of times Memphis will retransmit the same message to the same consumer

{% hint style="info" %}
For more information about how to create and connect a consumer to Memphis,&#x20;

please head [here](broken-reference)
{% endhint %}

### Supported Protocols

* NATS Protocol (Client SDKs)
* HTTP \* Soon \*
* WebSockets \* Soon \*
* gRPC \* Soon \*
* WASM \* Soon \*
* MQTT \* Soon \*
