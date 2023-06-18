---
description: This section describes the differeneces between Pulsar and Memphis
---

# Apache Pulsar vs Memphis

## What is Apache Pulsar?

Apache Pulsar is an open-source distributed messaging system. Originally developed as a queuing system, it has been broadened in recent releases to add event streaming features. Pulsar makes use of Apache BookKeeper™ for its storage layer—a project created at Yahoo as a high-availability solution to Hadoop's HDFS NameNode (although not ultimately used for that use case). It shares properties with both Kafka and RabbitMQ. Pulsar is a largely community-led project with no enterprise-grade commercial backing today.

## **What is Memphis.dev?**

**Memphis.dev** is a next-generation alternative to traditional message brokers.

It enables building modern queue-based applications that require large volumes of streamed and enriched data, modern protocols, zero ops, up to x9 faster development, up to x46 fewer costs, and significantly lower dev time for data-oriented developers and data engineers.

Low footprint, highly resilient, cloud-native, and run on any Kubernetes, on any cloud.

## General

| Parameter                 | Memphis.dev          | Apache Pulsar                             |
| ------------------------- | -------------------- | ----------------------------------------- |
| License                   | BSL 1.0              | Apache 2.0                                |
| Components                | Memphis + PostgreSQL | Pulsar + Zookeeper + BookKeeper + RocksDB |
| Message consumption model | Pull                 | Push                                      |
| Storage architecture      | Log                  | Index                                     |

## Ecosystem and User Experience

| Parameter                            | Memphis.dev                                                                                         | Apache Pulsar                                                                                                                                                                                     |
| ------------------------------------ | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Deployment                           | Made through `helm` or `docker compose`. Made out of two main components: broker and metadata store | Requires four components be configured and understood: Pulsar servers (brokers) plus Apache BookKeeper (bookies) plus Apache ZooKeeper servers as well as the RocksDB database used by BookKeeper |
| Enterprise support                   | Yes                                                                                                 | Yes                                                                                                                                                                                               |
| Managed cloud offerings              | Yes                                                                                                 | Yes                                                                                                                                                                                               |
| Self-Healing                         | Yes                                                                                                 | No                                                                                                                                                                                                |
| Notifications                        | Yes                                                                                                 | No                                                                                                                                                                                                |
| Message tracing (aka Stream lineage) | Yes                                                                                                 | No                                                                                                                                                                                                |
| Storage balancing                    | Automatic based on policy                                                                           | No                                                                                                                                                                                                |

## Availability and Messaging

| Parameter                      | Memphis.dev                 | Apache Pulsar                  |
| ------------------------------ | --------------------------- | ------------------------------ |
| Mirroring (Replication)        | Yes                         | Yes                            |
| Multi-tenancy                  | Yes                         | Yes                            |
| Ordering guarantees            | Consumer group level        | Partition level                |
| Storage tiering                | Yes                         | No                             |
| Permanent storage              | Yes                         | Messages get deleted after ACK |
| Delivery guarantees            | At least once, Exactly once | Exactly once                   |
| Idempotency                    | Yes                         | Yes                            |
| Geo-Replication (Multi-region) | Yes                         | Yes                            |

## Added Features

| Parameter                   | Memphis.dev                   | Apache Pulsar         |
| --------------------------- | ----------------------------- | --------------------- |
| GUI                         | Native                        | 3rd Party             |
| Dead-letter Queue           | Yes                           | No                    |
| Schema Enforcement          | Yes                           | Yes                   |
| Message routing             | Yes                           | Yes                   |
| Log compaction              | Yes                           | Yes                   |
| Message replay, time travel | Yes                           | Yes                   |
| Stream Enrichment           | SQL and custom code functions | SQL-based functions   |
| Pull retry mechanism        | Yes                           | Client responsibility |
