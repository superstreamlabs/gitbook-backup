---
description: >-
  This section describes the implementation of the Dead-letter station in
  Memphis.
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Dead-letter Station (DLS)

## Introduction

Dead-letter stations, also referred to as "Dead-letter queues" in some messaging systems, serve as both a concept and a practical solution essential for client debugging. They enable you to effectively isolate and "recycle" unconsumed messages, rather than discarding them, to diagnose why their processing was unsuccessful.

Upon the creation of a Memphis station, a default dead-letter station is established. It remains resource and storage-free until messages accumulate within it.

After the user has successfully debugged the consumer application or when the consumer application becomes available to process the messages, retransmission can occur directly from the dead-letter station using the following methods:

1. Move the messages back to the source station with a simple click in the Memphis GUI.
2. Retransmit the message directly to the intended consumer that initially failed to consume it.
3. Consume the message directly through the SDK from any consumer.
4. Optionally, drop the message from the Dead-letter station.

## What are "dead-letter" messages?

**"Dead-letter" messages** are defined as messages that lead to a consumer group continuously demanding delivery, possibly due to a consumer failure, preventing the message from being fully processed and acknowledged. Consequently, these messages persistently reoccur in the same consumer's queue.

Example: Consider a scenario where a message from an arbitrary station is retrieved by a consumer belonging to a specific consumer group. For various reasons, this consumer fails to process the message, whether due to a software bug, an unrecognized schema, resource constraints, or other factors.

## How do dead-letter stations work?

Sometimes, messages can't be processed because of various possible issues, such as erroneous conditions within the producer or consumer application, bad schema, or an unexpected state change that causes an issue with your application code. For example, if a user places a web order with a particular product ID, but the product ID is deleted, the web store's code fails and displays an error, and the message with the order request is sent to a dead-letter queue.

Occasionally, producers and consumers might fail to interpret aspects of the protocol that they use to communicate, causing message corruption or loss. Also, the consumer's hardware errors might corrupt the message payload or break the consumer itself.

A message will be flagged as "Poison" and sent to the dead-letter station **when passing the** `maxMsgDeliveries` **value.**

`maxMsgDeliveries` is the parameter that defines how many times the broker will try to redeliver the same message to the same CG until receiving an "Ack."

<figure><img src="../../.gitbook/assets/dls.jpeg" alt=""><figcaption></figcaption></figure>

## Types of messages

The DLS will automatically (based on user decision) catch messages of the following types -

* **Unacknowledged**. Messages that passed the `maxMsgDeliveries` parameter.
* **Schema violation.** Messages that did not pass the attached schema validation. As Memphis mission is to narrow data loss, and increase observability, messages that did not pass schema validation can be important and indicate some producer issues. Therefore, Memphis supports storing such messages.

<img src="../../.gitbook/assets/Screen Shot 2023-01-07 at 21.10.04.png" alt="" data-size="original">

<figure><img src="../../.gitbook/assets/schemaverse.jpeg" alt=""><figcaption><p>How "Schema violation" messages reach DLS</p></figcaption></figure>

### How to recover (=consume) a Dead-letter message

#### Using the GUI

Recovering or "resending" a dead-letter message can be accomplished without any code modifications or causing downtime for the consumers. Memphis facilitates this by retransmitting the message over the same "station connection."

Each type of stored dead-letter station (DLS) message is associated with a distinct recovery strategy:

* For "Unacknowledged" messages: The message is retransmitted directly to the unacknowledged consumer group(s). This approach differs from traditional message brokers, which typically return the message to the queue, potentially leading to already acknowledged messages for all consumer groups, including those that have already acknowledged the message and those that have not, thereby causing duplicate processing. Memphis employs a mechanism to resend the message exclusively to unacknowledged consumer groups, mitigating the risk of duplicate processing.
* For "Schema violation" messages: Currently, the only action available is observability. Message resend is not supported at present. Future releases will empower users to modify the schema if necessary and retransmit the message to the station if it has not been consumed by any consumer group.

#### Using the SDK

If you wish to consume all your dead-letter messages, you should establish a second station and establish a connection between the designated station and the new one to facilitate dead-letter message consumption.

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-06 at 16.42.34.png" alt=""><figcaption></figcaption></figure>

After establishing the link, dead-letter messages will be moved to the specified station, allowing Consumer Groups (CGs) to consume them.

It's important to note that the designated dead-letter station functions as a standard station and can be utilized accordingly.

### Message Journey

![Message Journey](../../.gitbook/assets/3.jpg)

After clicking on "Message Journey," the user will be redirected to this screen which is in the context of a single message.

**Left panel -** The producer of the message

**Center panel -** The message and its metadata.

**Right panel -** the unacknowledged consumer groups.
