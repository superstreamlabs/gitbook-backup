---
description: Offload stored messages to Amazon S3
cover: ../../../.gitbook/assets/Amazon S3 + Memphis.jpeg
coverY: 0
---

# Amazon S3

## Introduction

The common pattern of message brokers is to delete messages after passing the defined retention policy, like time/size/number of messages.\
Memphis offers a 2nd storage tier for longer, possibly infinite retention for stored messages.\
Each message that expels from the station will automatically migrate to the 2nd storage tier.\


