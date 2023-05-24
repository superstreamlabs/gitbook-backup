---
description: This section describes what are delayed messages and how to create it.
---

# Delayed messages

## Introduction

Delayed messages let you "return" a received message to the broker for a period when your consumer application needs additional time to process messages.

The uniqueness of Memphis implementation is&#x20;

1. The ability of the consumer to control the delay.
2. The number of unacked messages within the station does not affect the delayed message's consumption. For example, if a delay of 60 seconds is needed, this is exactly the amount of invisibility time that will be configured for that message.

## Flow

1. Consumer group receives a message
2. Some event caused the consumer group to hold the processing of the message
3. Given the `maxMsgDeliveries` has not reached its max value, the consumer will trigger \
   `message.delay(delayInMilliseconds)` , ignore the message, and instead of having the same message right after the first try, the broker will hold the message for the requested period of time
4. The next message will be consumed
5. After crossing the requested `delayInMilliseconds` the broker will stop the main stream of messages and push the delayed message again

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
})();ava
```
