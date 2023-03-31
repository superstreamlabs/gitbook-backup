---
description: This section describes what are delayed messages and how to create it.
---

# Delayed messages

## Introduction

Delayed messages let you "return" a received message back to the broker for a period of time when your consumer application needs additional time to process messages.

## Flow

1. Consumer group receives a message
2. Some event caused the consumer group to hold the processing of the message
3. Given the `maxMsgDeliveries` has not reached its max value, the consumer will trigger \
   `message.delay(delayInMilliseconds)` , ignore the message, and instead of having the same message right after the first try, the broker will hold the message for the requested period of time
4. The next message will be consumed
5. After crossing the requested `delayInMilliseconds` the broker will stop the main stream of messages and push the delayed message again

