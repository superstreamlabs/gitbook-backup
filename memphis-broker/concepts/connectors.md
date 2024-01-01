---
description: Extend your Memphis with connectors!
cover: ../../.gitbook/assets/Memphis connectors.jpeg
coverY: 0
---

# Connectors

## Introduction

Memphis Connectors allows developers to efficiently move data between various databases, streaming platforms, systems, and applications. Memphis.dev offers a user-friendly interface and a set of pre-built connectors, making it easier for developers to integrate different data sources and sinks without writing extensive code or clients.&#x20;

Its scalable architecture ensures that it can handle large volumes of data, and its flexibility allows for customization to meet specific business requirements. By leveraging Memphis.dev, organizations can achieve more seamless and effective data integrations, leading to better data management, transactional, machine-to-machine communication, and analysis capabilities.

Memphis Connectors are available to [Cloud](https://cloud.memphis.dev) users only.

### Combining connectors and functions

The integration of connectors and functions offers remarkable capabilities. An exemplary case would be establishing a connection from a Memphis station to a Kafka Topic.&#x20;

This setup involves initiating the ingestion of events at the partition level. Subsequently, a function is applied to transform, enrich, or filter these events. The processed events are then forwarded to another Kafka Topic or clients. This approach harnesses the unique power and superior developer experience provided by Functions, showcasing their efficiency and versatility in data handling and processing.

### Getting started

**Step 1:** To add a connector (source or sink), \
head to any station -> Click on the "+" button at the top of the producers/consumers panels -> Add a Source/Sink

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-13 at 17.16.35.png" alt=""><figcaption></figcaption></figure>

**Step 2:** Choose a direction and type

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-13 at 17.20.34.png" alt=""><figcaption></figcaption></figure>

### Catalog of Source Connectors

* Apache Kafka
* S3

### Catalog of Sink Connectors

* Apache Kafka
* S3
* Redis
* Memphis station
