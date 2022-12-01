# Table of contents

## 👉 Getting Started

* [Step 1 - Installation](README.md)
* [Step 2 - Hello World](getting-started/2-hello-world.md)
* [Use cases](getting-started/3-use-cases.md)
* [How to contribute?](getting-started/how-to-contribute.md)

## ⭐ Memphis

* [Overview](memphis/overview.md)
* [Architecture](memphis/architecture.md)
* [Concepts](memphis/concepts/README.md)
  * [Message broker](memphis/concepts/message-broker.md)
  * [Station](memphis/concepts/station.md)
  * [Producer API](memphis/concepts/producer.md)
  * [Consumer API](memphis/concepts/consumer.md)
  * [Consumer Group](memphis/concepts/consumer-groups.md)
  * [Storage and Redundancy](memphis/concepts/storage-and-redundancy.md)
  * [Authentication](memphis/concepts/security.md)
  * [Scaling](memphis/concepts/scaling.md)
  * [Ordering](memphis/concepts/ordering.md)
  * [Dead-letter Station (DLQ)](dashboard-ui/troubleshooting/dead-letter.md)
  * [Failover Scenarios](memphis/concepts/failover-scenarios.md)
  * [Idempotency (Duplicate processing)](memphis/concepts/idempotency.md)
* [Tiered Storage](memphis/tiered-storage.md)
* [Schemaverse - Schema Management](memphis/schemaverse-schema-management/README.md)
  * [Overview and Architecture](memphis/schemaverse-schema-management/overview-and-architecture.md)
  * [Formats](memphis/schemaverse-schema-management/formats.md)
  * [Getting Started](memphis/schemaverse-schema-management/getting-started.md)
  * [FAQs](memphis/schemaverse-schema-management/faqs.md)
* [Benchmark](memphis/benchmark.md)
* [Cluster configuration](memphis/cluster-configuration.md)
* [Comparisons](memphis/comparisons/README.md)
  * [Redpanda vs Memphis](memphis/comparisons/redpanda-vs-memphis.md)
  * [NATS vs Memphis](memphis/comparisons/nats-vs-memphis.md)
  * [Pulsar vs Memphis](memphis/comparisons/pulsar-vs-memphis.md)
  * [Apache Kafka vs Memphis](memphis/comparisons/apache-kafka-vs-memphis.md)
  * [RabbitMQ vs Memphis](memphis/comparisons/rabbitmq-vs-memphis.md)
* [Roadmap](memphis/roadmap.md)
* [Privacy](memphis/privacy.md)

## 📦 Deployment

* [Cloud Deployment](deployment/cloud-deployment/README.md)
  * [Deploy on AWS](deployment/cloud-deployment/deploy-on-aws.md)
  * [Deploy on GCP](deployment/cloud-deployment/deploy-on-gcp.md)
  * [Deploy on DigitalOcean](deployment/cloud-deployment/deploy-on-digitalocean.md)
  * [Deploy on Azure](deployment/cloud-deployment/deploy-on-azure.md)
* [Kubernetes](deployment/kubernetes/README.md)
  * [1 - Installation](deployment/kubernetes/1-installation.md)
  * [2 - Access](deployment/kubernetes/2-access.md)
* [Docker](deployment/docker-compose.md)

## SDKs

* [Node.js / Typescript / NestJS](sdks/node.js-typescript/README.md)
  * [Example: Producer.js](sdks/node.js-typescript/example-producer.js.md)
  * [Example: Producer.ts](sdks/node.js-typescript/example-producer.ts.md)
  * [Example: Consumer.js](sdks/node.js-typescript/example-consumer.js.md)
  * [Example: Consumer.ts](sdks/node.js-typescript/example-consumer.ts.md)
  * [Example: Producer.module.ts](sdks/node.js-typescript/example-producer.ts-1.md)
  * [Example: Consumer.controller.ts](sdks/node.js-typescript/example-producer.ts-2.md)
* [Go](sdks/go/README.md)
  * [Example: Producer](sdks/go/example-producer.md)
  * [Example: Consumer](sdks/go/example-consumer.md)
* [Python](sdks/python/README.md)
  * [Example: Producer](sdks/python/example-producer.md)
  * [Example: Consumer](sdks/python/example-consumer.md)
* [Rust](sdks/rust.md)
* [.NET](sdks/.net.md)
* [Java](sdks/java.md)
* [Scala](sdks/scala.md)

## Dashboard (UI)

* [General](dashboard-ui/general.md)
* [Signup](dashboard-ui/signup.md)
* [Login](dashboard-ui/login.md)
* [Overview](dashboard-ui/overview.md)
* [Stations](dashboard-ui/stations.md)
* [Troubleshooting](dashboard-ui/troubleshooting/README.md)
  * [System Logs](dashboard-ui/troubleshooting/system-logs.md)
* [Integrations](dashboard-ui/integrations/README.md)
  * [Notifications](dashboard-ui/integrations/notifications/README.md)
    * [Slack](dashboard-ui/integrations/notifications/slack.md)
* [Users](dashboard-ui/users.md)
* [Settings](dashboard-ui/settings.md)

## CLI

* [Installation](cli/installation.md)
* [Usage](cli/usage.md)

## 🗒 Release notes

* [How to upgrade memphis](release-notes/how-to-upgrade.md)
* [Releases](release-notes/releases/README.md)
  * [v0.4.1 - beta](release-notes/releases/v0.4.1-beta.md)
  * [v0.4.0 - beta](release-notes/releases/v0.4.0-beta.md)
  * [v0.3.6 - beta](release-notes/releases/v0.3.6-beta.md)
  * [v0.3.5 - beta](release-notes/releases/v0.3.5-beta.md)
  * [v0.3.0 - beta](release-notes/releases/v0.3.0-beta.md)
  * [v0.2.2 - beta](release-notes/releases/v0.2.2-beta.md)
  * [v0.2.1 - beta](release-notes/releases/v0.2.1-beta.md)
  * [v0.2.0 - beta](release-notes/releases/v0.2.0-beta.md)
  * [v0.1.0 - beta](release-notes/releases/v0.1.0-beta.md)
