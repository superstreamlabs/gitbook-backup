---
description: >-
  A comparison article between Schemaverse, Confluent Schema Registry, and AWS
  Glue
---

# Comparison

## Introduction to schemas

Before delving into the different supporting technologies, let's establish a baseline understanding of schemas and their role in message brokers or async server-server communication.

Schema = Structure.

Schema defines the structure of a "message" and follows a specific format to ensure effective communication between different applications/services/electronic entities.

Schemas can be found in both SQL & NoSQL databases, providing the structure expected by the database to receive data. For example, `first\_name:string` or `first.name` etc. An unfamiliar or noncompliant schema will result in data being dropped, and the database will not save the record. 

Likewise, schemas are critical in communication between two logical entities, such as two microservices. Consider a scenario where Service A writes a message to Service B, which expects a specific format like Protobuf, and its logic or code depends on specific keys and value types. Even a simple typo in a column name or an unexpected schema or format can cause issues for the consumer.

Schemas serve as a manual or automatic contract that ensures stable communication and dictates how two entities should interact.

The following comparison of technologies will help you establish and enforce schemas between services as data flows from one service to another.

## What is AWS Glue?

AWS Glue is a serverless data integration service that makes it easier to discover, prepare, move, and integrate data from multiple sources for analytics, machine learning (ML), and application development.

<figure><img src="https://lh5.googleusercontent.com/SWdNvxRqsE7DnjS5nUtkXphyZKP16lHNlmoHJKrhJnHKbk4LkTyxUNM-ENKtO-AqKvPxfXaI9hjwvqUZ83n6fm1NOE8EjxHqVMxOseUSuIc_T75FdHdM_NwPFOBcUZ1rKF5-ofno9MO8XRBgLNoldFU" alt=""><figcaption><p>Credit: https://aws.amazon.com/glue/</p></figcaption></figure>

### Capabilities

* Data integration engine
* Event-driven ETL
* No-code ETL jobs
* Data preparation

Main components of AWS Glue are the Data Catalog which stores metadata, and an ETL engine that can automatically generate Scala or Python code. Common data sources would be Amazon S3, RDS, and Aurora.

## What is Confluent Schema Registry?

Confluent Schema Registry provides a serving layer for your metadata.&#x20;

It provides a RESTful interface for storing and retrieving your Avro®, JSON Schema, and [Protobuf](https://developers.google.com/protocol-buffers/) [schemas](https://docs.confluent.io/platform/current/schema-registry/schema\_registry\_onprem\_tutorial.html#schema-registry-tutorial-definition).&#x20;

It stores a versioned history of all schemas based on a specified subject name strategy, provides multiple compatibility settings and allows evolution of schemas according to the configured compatibility settings and expanded support for these schema types.

It provides serializers that plug into Apache Kafka® clients that handle schema storage and retrieval for Kafka messages that are sent in any of the supported formats.

<figure><img src="https://lh4.googleusercontent.com/TsEE5GMwkbMLRKzv51BG6KoL9GY_Eh_ZceRacC5XOgMP_pgQY6GNKIil4-G1tXECXW8SYzlsqjQwU6i1Q6aeZygDmgCNzeeN1YlmjjiuXggpBIsdOX57XrMLedg3xsZYL8ARI9ftTaf3Mr7BkB5UplE" alt=""><figcaption><p>Credit: https://docs.confluent.io/platform/current/schema-registry/index.html#schemas-subjects-and-topics</p></figcaption></figure>

Schema Registry lives outside of and separately from your Kafka brokers.

Your producers and consumers still talk to Kafka to publish and read data (messages) to topics.&#x20;

Concurrently, they can also talk to Schema Registry to send and retrieve schemas that describe the data models for the messages.

## What is Memphis.dev Schemaverse?

Memphis Schemaverse provides a robust schema store and schema management layer on top of Memphis broker without a standalone compute unit or dedicated resources. \
With a unique & modern UI and programmatic approach, technical and non-technical users can create and define different schemas, attach the schema to multiple stations and choose if the schema should be enforced or not.&#x20;

Memphis' low-code approach removes the serialisation part as it is embedded within the producer library. Schemaverse supports versioning, GitOps methodologies, and schema evolution.

<figure><img src="https://lh5.googleusercontent.com/rcGgBPpPQld01KV2dtELxVL-w5gbDr5RaSM7Ax9HybS1x6UvsD8YQBLYlbiB1SoC5Mw5ANe8BKB7eK2OF1p4j6DNVMvz_TEtawqjJDPrSJPx1rclgt7I1Z7s2SPxzL4B4nFCXLhPTXApSmu4F81xOtk" alt=""><figcaption></figcaption></figure>

Schemaverse's main purpose is to act as an automatic gatekeeper and ensure the format and structure of ingested messages to a Memphis station and to reduce consumer crashes, as often happens if certain producers produce an event with an unfamiliar schema.

### Current version common use cases

* Schema enforcement between microservices
* Data contracts
* Convert events’ format
* Create an organisational standard around the different consumers and producers

## Comparison

| Parameter                  | AWS Glue                                                                                              | Schema Registry                                            | Schemaverse                                                       |
| -------------------------- | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ----------------------------------------------------------------- |
| Data formats               | JSON Schema, Avro, Protobuf                                                                           | Avro, JSON Schema, Protobuf                                | JSON Schema, Protobuf, GraphQL                                    |
| Validation and Enforcement | Yes                                                                                                   | Yes                                                        | Yes                                                               |
| Serialisation              | Requires implementation                                                                               | Requires implementation                                    | Transparent                                                       |
| Deserialization            | Requires implementation                                                                               | Requires implementation                                    | Transparent                                                       |
| Management interface       | GUI, CLI, SDK                                                                                         | REST, SDK, GUI                                             | SDK, GUI, CLI                                                     |
| Supported languages        | Scala                                                                                                 | Java, .NET, Python                                         | Go, Node.js, Python, REST, TypeScript, NestJS, Java, .NET, Kotlin |
| Compatibility mode         | backward or forward                                                                                   | backward or forward                                        | backward or forward                                               |
| Schema creation            | Manual / Auto                                                                                         | Manual / Auto                                              | Manual                                                            |
| Pricing                    | $1.00 per 100,000 objects stored above 1M, per month + $1.00 per million requests above 1M in a month | Confluent Community License / Confluent Enterprise licence | Open-source / Free                                                |

### Validation and Enforcement

When data streaming applications are integrated with a schema management, schemas used for data production are validated against schemas within a central registry, allowing you to control data quality centrally.

**AWS Glue** offers enforcement and validation using Glue schema registry for Java-based applications using Apache Kafka, AWS MSK, Amazon Kinesis Data Streams, Apache Flink, Amazon Kinesis Data Analytics for Apache Flink, and AWS Lambda.&#x20;

[https://docs.aws.amazon.com/glue/latest/dg/schema-registry-gs.html](https://docs.aws.amazon.com/glue/latest/dg/schema-registry-gs.html)

**Schema registry** validates and enforces messages schemas at both the client and server side. Validation will take place at client side, and by performing a serialisation over the about-to-be-produced data by retrieving the schema from schema registry. Confluent provides read-to-use serialisation functions that can be used. Schema updates and evolution will require to boot the client and fetch the updates, so as changing the schema at the registry level. It first required to be switched into certain mode (forward/backward), perform the change, and than bring back to default.

**Schemaverse** validates and enforces the schema at the client level as well without the need for manual schema fetch, and supports runtime evolution, meaning clients dont need a reboot to apply new schema changes, including different data formats.

Schemaverse also makes the serialisation/deserialization transparent to the client and embeds it within the SDK based on the required data format.

### Serialization/Deserialization

When sending data over the network it needs to be encoded into bytes before.

AWS Glue and Schema Registry works similarly. Each created schema has an ID.\
When the application producing data has registered its schema, the Schema Registry serializer validates that the record being produced by the application is structured with the fields and data types matching a registered schema.&#x20;

Deserialization will take place by a similar process by fetching the needed schema based on the given ID within the message.

In AWS Glue and Schema Registry, It is the client responsibility to implement and deal with the serialisation while in Schemaverse it is fully transparent and all is needed by the client is to produce a message that complies with the required structure.

\
