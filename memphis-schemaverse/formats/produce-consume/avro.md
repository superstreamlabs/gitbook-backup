---
description: Soon to be supported.
---

# Avro

## Introduction

[Avro](https://avro.apache.org/) is a row-oriented remote procedure call and data serialization framework developed within Apache's Hadoop project. It uses JSON for defining data types and protocols and serializes data in a compact binary format. Its primary use is in Apache Hadoop, where it can provide both a serialization format for persistent data, and a wire format for communication between Hadoop nodes, and from client programs to the Hadoop services. Avro uses a schema to structure the data that is being encoded. It has two schema languages; one for human editing (Avro IDL) and another more machine-readable based on JSON.

Help us prioritize Avro. Upvote here [https://github.com/memphisdev/memphis/issues/1061](https://github.com/memphisdev/memphis/issues/1061)

## Produce/Consume a message

{% tabs %}
{% tab title="Node.js" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

In node.js, we can simply produce an object. Behind the scenes, the object will be serialized based on the attached schema and data format.

**Example schema:**

{% code lineNumbers="true" %}
```json
```
{% endcode %}

**Code:**

{% code lineNumbers="true" %}
```javascript
```
{% endcode %}
{% endtab %}
{% endtabs %}
