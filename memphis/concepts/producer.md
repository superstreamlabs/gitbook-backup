---
description: Page under construction
---

# Producer API

### What is a producer?

A producer is the source application/service that pushes data or messages to the broker or more specifically to the station.&#x20;

As the user configures a client connection to Memphis, it comprises several objects

* Connection - An open socket between the client to Memphis. Only required once as the client/application gets initialized for the first time.
* Producer - A producer entity must be declared to write data/messages into Memphis.
* (And/Or) Consumer - A consumer entity must be declared to read data/messages from Memphis.

<figure><img src="../../.gitbook/assets/Producer.jpeg" alt=""><figcaption></figcaption></figure>

### Parameters

(\*) Names might be a bit different from one SDK to another. Meanings are the same.

**Connection**

* host: Memphis URL
* port: Memphis port
* username: Can be root or any other application-type user
* connectionToken: The token received when the user created. Will change in the future to more robust credentials and authentication system
* reconnect: The connection entity will try to reconnect to Memphis in case of a disconnection
* maxReconnect: Amount of time the client will try to reconnect before backing off
* reconnectIntervalMs: Time window between one retry to another
* timeoutMs: Ability to kill a dead connection after explicit time

**Producer**

```
const producer = await memphisConnection.producer({
            stationName: "<station-name>",
            producerName: "<producer-name>"
        });
```



### How to create a producer?

### Scale considerations

