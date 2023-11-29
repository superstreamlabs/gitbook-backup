---
cover: ../.gitbook/assets/Github.png
coverY: 0
---

# Use cases and best practices

## Memphis.dev Well-Architected

Learn, get inspired, and build better pipelines using architectural best practices.

### Use case: Apply and enforce schemas over Kafka topics

<figure><img src="../.gitbook/assets/Card 1 - Apply schemas to Kafka Topics. (1).jpg" alt=""><figcaption></figcaption></figure>

Memphis Functions works great next to Kafka and simplifies the needed code around it.\
In this use case, Memphis Functions acts as a consumer group to a certain Kafka topic (can be for specific partitions).

* Each received event gets consumed by Functions CG, goes through schema validation defined in [Schemaverse](broken-reference)
* Validated events continue their journey to the next Topic by Functions. Unvalidated events will be dropped, enter Memphis Dead-letter and the user will be notified through email or Slack
* Although we did add an extra hop, both the movement and validation take place quickly and efficiently + separating raw, unvalidated data from the validated one in two different topics, creating an extra layer of insurance for the consumers

Use case
