---
description: What is it? And how to avoid it.
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Idempotency (Duplicate processing)

How to avoid duplicate producing and consuming the same message more than once.

## What is Idempotency?

Idempotence, in programming and mathematics, is a calculation of some operations such that no matter how many times you execute them, you achieve the same result.

In a streaming environment, retrying to send a failed message often includes a small risk that both messages were successfully written to the broker, leading to duplicates.

### The problem with retries

On the producer side, this can happen as illustrated below.

1. The producer sends a message to Memphis
2. The message was successfully written and stored
3. Network issues prevented the broker acknowledgment from reaching the producer
4. The producer will treat the lack of acknowledgment as a temporary network issue and retry sending the message (since it can’t know it was received).
5. In that case, the broker will end up having the same message twice.

<figure><img src="../../.gitbook/assets/idempotence 1 (1).jpeg" alt=""><figcaption></figcaption></figure>

On the consumer side, this can happen as illustrated below.

1. Consumer group consume message from Memphis
2. Process the message
3. A sudden issue like missing resources/network failure made the CG not return an acknowledgment to the broker
4. Based on `maxAckTimeMs` parameter Memphis broker decides to retransmit the same message
5. The consumer will process the same message again

<figure><img src="../../.gitbook/assets/idempotence 2.jpeg" alt=""><figcaption></figcaption></figure>

## Producer side - How to avoid?

**Producer idempotence** ensures that duplicate messages are not introduced due to unexpected retries.

With an idempotency producer, the process will take place as illustrated below.

<figure><img src="../../.gitbook/assets/idempotence producer.jpeg" alt=""><figcaption></figcaption></figure>

### **How does it work internally?**

1. The producer sets a unique ID for each message
2. The SDK attaches the ID to the message
3. Inside the station, there is a table stored in the cache that stores all the ingested IDs
4. In case a message gets produced with an ID that is already stored in that table, it will be dropped
5. The table resets every defined time as configured upon station creation

### Step 1: Set up idempotency cache time

Via the GUI during station creation.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-30 at 12.18.38.png" alt=""><figcaption><p>GUI example</p></figcaption></figure>

Or via the different SDKs.

```
idempotencyWindowMs: 0, // defaults to 120000
```

As explained in "[How does it work internally?](idempotency.md#how-does-it-work-internally)", the timer is responsible for the retention of the messages IDs table. For example, if 3 hours are chosen, the table will be emptied every three hours.

### Step 2: Set up messages IDs

Producer becomes idempotence once adding IDs to the messages.

For example:

```
await producer.produce({
    message: '<bytes array>/object', // Uint8Arrays / You can send object in case your station is schema validated
    ackWaitSec: 15, // defaults to 15
    msgId: "fdfd" // defaults to null
});
```

## Consumer group side - How to avoid?

**Consumer idempotence** ensures that duplicate messages are not introduced due to unexpected retries.

To avoid the situation, it is recommended to use idempotence producers and set `maxMsgDeliveries` to 1 on the consumer side.

By configuring `maxMsgDeliveries` to 1, in a sudden failure of the consumer in a CG, the entire CG will not receive the same message again, and it will be stored automatically in the [DLS](../../dashboard-ui/troubleshooting/dead-letter.md) for supervised retries.

Search terms: Consumed multiple times, duplicate message
