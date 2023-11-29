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
In this scenario, it operates as a consumer group for a specific Kafka topic, potentially targeting specific partitions.

* Functions CG consumes each received event and subjects it to schema validation as defined in [Schemaverse](broken-reference).
* After validation, approved events proceed to the next Topic through Functions. Unapproved ones are discarded, directed to the Memphis Dead-letter, and trigger user notifications via email or Slack.
* Despite introducing an additional step, both the movement and validation occur rapidly and efficiently. This segregation of raw, unvalidated data and validated data into separate topics adds an extra protective layer for consumers.

### Use case: Prepare and enrich traces before entering BigQuery

<figure><img src="../.gitbook/assets/Card 2 - Prepare and enrich user traces (1).jpg" alt=""><figcaption></figcaption></figure>

Preparing real-time data presents a significant challenge when inserting it into BQ due to the necessary transformations before aligning with the specified schema.&#x20;

One approach involves crafting a substantial client that consumes events, parses their types, and performs corresponding transformations. However, this method lacks scalability, and any code modifications may disrupt others due to its monolithic nature. Moreover, the constant changes in schemas, tables, and requirements exacerbate these challenges.

Memphis Functions enable the distribution of logic, enabling the creation of dedicated pipelines for each event type across multiple stations.

**How?**

* Memphis producers can produce the same message to multiple stations [simultaneously](../memphis/concepts/producer.md#produce-messages-to-multiple-stations-simultaneously).
* A dedicated function sequence will be attached for each station.

**Benefits**

* Individual handling for each event type
* Fully distributed and highly decoupled
* Processing issues won't impact all pipelines
