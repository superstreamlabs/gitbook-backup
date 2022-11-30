---
description: What is it? And how to avoid it.
---

# Idempotency (Duplicate processing)

How to avoid duplicate producing and consuming of the same message more than once

## What is Idempotency?

Idempotence, in programming and mathematics, is a calculation of some operations such that no matter how many times you execute them, you achieve the same result.

In a streaming environment, retrying to send a failed message often includes a small risk that both messages were successfully written to the broker, leading to duplicates.

### The problem with retries

On the producer side, this can happen as illustrated below.

1. Producer sends a message to Memphis
2. The message was successfully written and stored
3. Network issues prevented the broker acknowledgment from reaching the producer
4. The producer will treat the lack of acknowledgment as a temporary network issue and retry sending the message (since it can’t know it was received).
5. In that case, the broker will end up having the same message twice.

<figure><img src="../../.gitbook/assets/idempotence 1.jpeg" alt=""><figcaption></figcaption></figure>

On the consumer side, this can happen as illustrated below.

1. Consumer group consume message from Memphis
2. Process the message
3. A sudden issue like missing resource/network failure made the CG not return acknowledgment
4. Based on `maxAckTimeMs` parameter Memphis decides to retransmit the same message
5. The consumer will process the same message again

<figure><img src="../../.gitbook/assets/idempotence 2.jpeg" alt=""><figcaption></figcaption></figure>

## Producer side - How to avoid?

Producer idempotence ensures that duplicates are not introduced due to unexpected retries.



<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-30 at 12.18.38.png" alt=""><figcaption></figcaption></figure>

With an idempotency producer, the process will take place as illustrated below.

<figure><img src="../../.gitbook/assets/idempotence producer.jpeg" alt=""><figcaption></figcaption></figure>

### **How does it work internally?**

1. The producer&#x20;

### Step 1: Set up idempotency cache time

Via the GUI or SDK, during station creation.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-30 at 12.18.38.png" alt=""><figcaption><p>GUI</p></figcaption></figure>

Search terms: Consumed multiple times, duplicate message
