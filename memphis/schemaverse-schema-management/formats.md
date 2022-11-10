---
cover: ../../.gitbook/assets/schema - formats.jpeg
coverY: 0
---

# Formats

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-08 at 22.10.44.png" alt=""><figcaption></figcaption></figure>

This section describes the formats that Memphis Schemaverse currently supports or in the future.

### Google Protobuf (Released)

Protocol Buffers (Protobuf) is a free and open-source cross-platform data format used to serialize structured data, Initially released on July 7, 2008. It is useful in developing programs to communicate with each other over a network or for storing data. The method involves an interface description language that describes the structure of some data and a program that generates source code from that description for generating or parsing a stream of bytes that represents the structured data.

#### Supported versions

* proto2
* proto3

#### Supported Features

* Versioning
* Embedded serialization
* Producer Live evolution
* Import packages (soon)
* Import types (soon)

### Apache Avro (Coming in v0.4.1)

Avro is a row-oriented remote procedure call and data serialization framework developed within Apache's Hadoop project. It uses JSON for defining data types and protocols and serializes data in a compact binary format. Its primary use is in Apache Hadoop, where it can provide both a serialization format for persistent data, and a wire format for communication between Hadoop nodes, and from client programs to the Hadoop services. Avro uses a schema to structure the data that is being encoded. It has two schema languages; one for human editing (Avro IDL) and another more machine-readable based on JSON.

### JSON (Coming in v0.4.1)

JSON Schema is a vocabulary that allows you to annotate and validate JSON documents.\
It provides clear human- and machine-readable documentation and offers data validation which is useful for Automated testing. Ensuring quality of client-submitted data.

### GraphQL (Coming in v0.5.0)

GraphQL is an open-source data query and manipulation language for APIs, and a runtime for fulfilling queries with existing data. GraphQL was developed internally by Facebook in 2012 before being publicly released in 2015. On 7 November 2018, the GraphQL project was moved from Facebook to the newly established GraphQL Foundation, hosted by the non-profit Linux Foundation.

#### Supported features

* a

