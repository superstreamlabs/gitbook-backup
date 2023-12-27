---
description: >-
  What is an exchange? What are routing keys? How are exchanges and stations
  associated with each other? When should I use them and how? This page explains
  it all
---

# Data exchange

Exchange is a well-familiar concept in RabbitMQ and refers to a mechanism where a message is sent to a kind of router instead of directly to some queue. This router then directs the message to a specific queue, depending on a defined policy or, more precisely, based on a routing key.

Exchange is an excellent tool for scenarios where each message needs to be streamed to a different destination based on certain conditions or payload.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Memphis provides the functionality to route specific messages to one or several stations, depending on predefined headers.

### How it works

#### Step 1: Assign a dedicated header to each message to indicate the specific station it should be directed to.

* This can be accomplished by implementing tagging at the producer level or through a tagging function, which adds tags and headers to each message according to a highly customizable policy.

#### Step 2: Set up a destination, designated as a type of station.
