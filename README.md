---
description: What is Memphis.dev? The challenges it solves and how
cover: .gitbook/assets/Docs (4).png
coverY: 0
---

# Introduction

## The Origin of Challenges

When your application demands a message broker or queue, embarking on this journey will entail:

1. Constructing a dead-letter queue while implementing observability and a retry system.
2. Establishing a scalable infrastructure.
3. Crafting client wrappers for integration.
4. Tagging events for multi-tenancy.
5. Enforcing schemas and handling transformations.
6. Addressing back pressure, whether on the client or queue side.
7. Configuring monitoring and real-time alerts.
8. Developing a cloud-agnostic solution.
9. Aligning configurations between the production and development environments.
10. Devoting weeks and months to mastering the intricacies via archived documentation, ebooks, and courses.
11. Bringing your developers on board.

And the list continues...

## So, What is Memphis.dev?

Memphis.dev is more than a broker. It's a new streaming stack.

Memphis.dev is a highly scalable event streaming and processing engine. Before Memphis came along, handling ingestion and processing of events on a large scale took months to adopt and was a capability reserved for the top 20% of mega-companies. Now, Memphis opens the door for the other 80% to unleash their event and data streaming superpowers quickly, easily, and without breaking the bank.

### The Core Elements of Memphis:

1. Memphis Broker: A distributed engine that serves as the primary storage layer for produced events and data.
2. Memphis Schemaverse: An integrated schema management and enforcement tool within Memphis, assisting users in enhancing data quality and preventing disruptions upstream.
3. Memphis Functions: A developer-centric, serverless stream processing feature for real-time event transformation and enrichment.
4. Memphis Connect: A modular framework designed to facilitate rapid data extraction and transfer between various sources and destinations.

<figure><img src=".gitbook/assets/Ways to Compare Different Event Sources (1).jpg" alt=""><figcaption></figcaption></figure>

### Key Features of Memphis.dev

1. Rock-Solid Reliability - Our queues and brokers are the backbone of today's applications, ensuring they are as available and stable as can be.
2. Blazing Performance and Efficiency - We deliver top-notch performance without guzzling up your resources.
3. Developer-Friendly - Memphis.dev makes development lightning-fast, getting your creations into production in no time.
4. Crystal-Clear Insight - Boost your observability with seamless integrations into third-party monitoring tools, real-time notifications, stream lineage, and slash troubleshooting time.

## Walkthrough

{% @storylane/embed subdomain="app" linkValue="py6hwmsyvgox" url="https://app.storylane.io/demo/py6hwmsyvgox" %}

## High-level diagram

<figure><img src=".gitbook/assets/overview (1).jpeg" alt=""><figcaption></figcaption></figure>

## Typical Applications

1. Handling Background Tasks
2. Real-Time Data Pipelines
3. Collecting Data
4. Asynchronous Communication for Microservices
5. Task Queues
6. Distributing Messages to Multiple Targets
7. Ingesting Grafana Loki Logs
8. Streaming Video Frames
