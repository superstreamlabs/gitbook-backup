---
description: Released on v0.4.0
cover: ../../.gitbook/assets/schema - overview.jpeg
coverY: 0
---

# Schemaverse

## Introduction

In a federated data platform, in which responsibilities are distributed between stakeholders, teams, and sources, it’s harder to control and establish a single standard. This is where the data contracts concept comes into play. Why do data contracts matter? Because they:
  
* Provide insights into who owns what data products
* Support setting standards and managing your data pipelines with confidence

 They also provide crucial information on what data is being consumed, by whom, and for what purpose. Bottom line: data contracts are essential for robust data management!

<figure><img src="../../.gitbook/assets/schema 1.jpeg" alt=""><figcaption></figcaption></figure>

The very basic building block to control and ensure the quality of data that flows through your organization between the different owners is by defining well-written schemas and data models.

### Why should you use schemas?

Data pipelines are constantly breaking, creating data quality and usability issues, and there is a communication chasm between service implementers, data engineers, and data consumers.

By defining a well-struct schema and enforcing it over your different data producers, you increase the quality of your data, lower the client logic needed to transform unstructured data, and decrease pipelines and consumer breaks.

## Meet Schemaverse!

* No code.
* No client reboots.&#x20;
* Runtime updates.
* Transparent serialization.

<figure><img src="../../.gitbook/assets/Schema Management Overview (2).jpg" alt=""><figcaption><p>Overview</p></figcaption></figure>

### Behind the stage

<figure><img src="../../.gitbook/assets/schemaverse.jpeg" alt=""><figcaption><p>Sequence diagram</p></figcaption></figure>

Memphis Schemaverse provides a robust schema store and schema management layer on top of memphis broker without a standalone compute or dedicated resources.&#x20;

With a unique and modern UI and programmatic approach, technical and non-technical users can create and define different schemas, attach the schema to multiple stations, and choose if the schema should be enforced or not.&#x20;

Memphis' low-code approach removes the serialization part as it is embedded within the producer library. Schemaverse supports versioning, GitOps methodologies, and schema evolution.

Also includes -&#x20;

* Great UI and programmatic approach Embed within the broker&#x20;
* Zero-trust enforcement&#x20;
* Versioning&#x20;
* Out-of-the-box monitoring&#x20;
* Import & Export schemas
* Low/no-code validation and serialization&#x20;
* No configuration needed
* Native support in Python, Go, Node.js

### Key features

* Runtime-level rendering of existing producers and consumers
* Unaligned messages will be dropped immediately as the schema gets attached
* Schemas can be modified without rebooting producers
* Producer-level validation - to reduce resources needed from the broker
* Supported SDKs: Go, Python, Node.js (Nest / Typescript)

### Supported formats&#x20;

A quick overview of the most popular formats supported by Schemaverse.

* Protobuf. \
  The modern. Protocol buffers are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data – think XML, but smaller, faster, and simpler.&#x20;
* Avro. \
  The popular. Apache Avro™ is the leading serialization format for record data and the first choice for streaming data pipelines. It offers excellent schema evolution.&#x20;
* JSON \
  The simplest. JSON Schema is a vocabulary that allows you to annotate and validate JSON documents.
* GraphQL\
  GQL is a query language for APIs and a runtime for fulfilling those queries with your existing data.

### The process

#### 1. Schema creation and enforcement

The very first step would be to create a schema, based on the required data model, and apply it over a station.&#x20;

When creating a schema, the creator must choose a data format that will also determine the data format of the ingested messages and several more characteristics. Each format has its own advantages, as described [here](formats/).

#### 2. Validation

After attaching a schema to a station, each producer will be obligated to fulfill that schema.

Once a producer gets connected to a station, **either before a schema got attached to the station or after**, the new schema will be retrieved **live,** and every new message that will be sent to Memphis station will be validated against the attached schema that will reside on the producer cache as well, constantly listening to updates.

Only valid messages that fit the attached schema will be able to enter the schema.

#### 3. Serialization process (No code needed)

Serialization is a process of converting a data object—a combination of code and data—into a series of bytes that saves the object's state in an easily transmittable form. The opposite process is called deserialization.&#x20;

Serialization is basically represented as a function, each data format with its own implementation, and in case the structure and content of a given data do not match the defined schema in the .proto/.avro/.json struct, the serialization process fails, and therefore, the message will not be sent to the broker.&#x20;

That process establishes the message's initial validation before it reaches the broker itself, using the client cache to store the schema locally.

<figure><img src="https://lh5.googleusercontent.com/9ifhev7freLnIYyD_Y3zmrgZAp9-2Bf8eYsSAps0N_77PblO4eG0LGodJY6C6bBmhCxYDRMocztYK3Sge8WMezMMrZFyODEBOw5YZ2xmB7xqqrkhJcds-f67XqHSXNTydr3PpcI2e09yze32L4h0_kg3CcZAxPepTFtJJ_oStF-myZdomFjy2t7XVxZf" alt=""><figcaption></figcaption></figure>

## Getting Started

For code producer/consumer code examples, choose a [data format](formats/).

## To consider

* Schema attachment can be disruptive because it will update producers (live) to drop messages that are not aligned with the attached schema.
