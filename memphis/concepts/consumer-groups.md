---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Consumer Group

### What is a consumer group?

A consumer group is a group of consumers, usually combine multiple clients the same characteristics and/or replicas/workers of the same application/client.

The added layer of a consumer group enables to control ordering and avoid duplicate processing of messages within the same type of consumers.

Each message will be delivered in parallel to all consumer groups.

<figure><img src="../../.gitbook/assets/consumer group.jpeg" alt=""><figcaption><p>Each consumer group gets the same messages</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/consumer group 2.jpeg" alt=""><figcaption><p>Inside the consumer group, a message will be delivered to only one consumer</p></figcaption></figure>

There is no consumer without a consumer group.
