---
description: This section describes what are delayed messages and how to create it.
---

# Delayed messages

## Introduction

Delayed messages allow you to send a received message back to the broker when your consumer application requires extra processing time.

What sets apart Memphis' implementation is the consumer's capability to control this delay independently and atomically.&#x20;

Within the station, the count of unconsumed messages doesn't impact the consumption of delayed messages. For instance, if a 60-second delay is necessary, it precisely configures the invisibility time for that specific message.

## Flow

1. A message is received by the consumer group.
2. An event occurs, prompting the consumer group to pause processing the message.
3. Assuming the `maxMsgDeliveries` hasn't hit its limit, the consumer will activate `message.delay(delayInMilliseconds)`, bypassing the message. Instead of immediately reprocessing the same message, the broker will retain it for the specified duration.
4. The subsequent message will be consumed.
5. Once the requested `delayInMilliseconds` has passed, the broker will halt the primary message flow and reintroduce the delayed message into circulation.

<figure><img src="../../.gitbook/assets/delayed queues.jpeg" alt=""><figcaption></figcaption></figure>

## Code example

```javascript
const { memphis } = require('memphis-dev');

(async function () {
    let memphisConnection;

    try {
        memphisConnection = await memphis.connect({
            host: '<memphis-host>',
            username: '<application type username>',
            connectionToken: '<broker-token>'
        });

        const consumer = await memphisConnection.consumer({
            stationName: '<station-name>',
            consumerName: '<consumer-name>',
            consumerGroup: ''
        });

        consumer.setContext({ key: "value" });
        consumer.on('message', (message, context) => {
            console.log("Delaying message for 1 minute");
            message.delay(60000);
        });

        consumer.on('error', (error) => { });
    } catch (ex) {
        console.log(ex);
        if (memphisConnection) memphisConnection.close();
    }
})();
```
