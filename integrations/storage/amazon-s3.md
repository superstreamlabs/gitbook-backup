---
description: Offload stored messages to Amazon S3
cover: ../../../.gitbook/assets/Amazon S3 + Memphis.jpeg
coverY: 0
---

# Amazon S3

## Introduction

The common pattern of message brokers is to delete messages after passing the defined retention policy, like time/size/number of messages.\
Memphis offers a 2nd storage tier for longer, possibly infinite retention for stored messages.\
Each message that expels from the station will automatically migrate to the 2nd storage tier.

## Getting Started

### Step 1: Configuring the integration

First, configure Amazon S3 at the integration center.\
It's a one-time configuration for an entire Memphis instance/cluster.

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-02-20 at 16.32.38.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-02-20 at 16.36.27.png" alt=""><figcaption></figcaption></figure>

### Step 2: Enable S3 per station

S3 can be used as a remote storage tier during station creation

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-02-20 at 16.48.26.png" alt=""><figcaption></figcaption></figure>
