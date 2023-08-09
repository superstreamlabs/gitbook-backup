# Table of contents

## 👉 Getting Started

* [Introduction](README.md)
* [Quick start](getting-started/2-hello-world.md)
* [Tutorials](getting-started/tutorials/README.md)
* [How others use Memphis](getting-started/public-case-studies.md)
* [How to contribute?](getting-started/how-to-contribute.md)
* [Roadmap](https://memphis.dev/roadmap)

## ☁ Memphis cloud

* [Getting Started](memphis-cloud/getting-started.md)
* [Pricing](https://memphis.dev/pricing)

## ⭐ Memphis Broker

* [Architecture](memphis/architecture.md)
* [Key concepts](memphis/concepts/README.md)
  * [Message broker](memphis/concepts/message-broker.md)
  * [Station](memphis/concepts/station.md)
  * [Producer API](memphis/concepts/producer.md)
  * [Consumer API](memphis/concepts/consumer.md)
  * [Consumer Group](memphis/concepts/consumer-groups.md)
  * [Storage and Redundancy](memphis/concepts/storage-and-redundancy.md)
  * [Security/Authentication](memphis/concepts/security.md)
  * [Scaling](memphis/concepts/scaling.md)
  * [Ordering](memphis/concepts/ordering.md)
  * [Dead-letter Station (DLS)](dashboard-ui/troubleshooting/dead-letter.md)
  * [Delayed messages](memphis/key-concepts/delayed-messages.md)
  * [Idempotency (Duplicate processing)](memphis/concepts/idempotency.md)
  * [Failover Scenarios](memphis/concepts/failover-scenarios.md)
  * [Troubleshooting process](memphis/key-concepts/troubleshooting-process.md)
* [Memphis configuration](memphis/memphis-configuration.md)
* [Comparisons](memphis/comparisons/README.md)
  * [NATS Jetstream vs Memphis](memphis/comparisons/nats-vs-memphis.md)
  * [RabbitMQ vs Memphis](memphis/comparisons/rabbitmq-vs-memphis.md)
  * [AWS SQS vs Memphis](memphis/comparisons/aws-sqs-vs-memphis.md)
  * [Apache Kafka vs Memphis](memphis/comparisons/apache-kafka-vs-memphis.md)
  * [Apache Pulsar vs Memphis](memphis/comparisons/apache-pulsar-vs-memphis.md)
  * [ZeroMQ vs Memphis](memphis/comparisons/zeromq-vs-memphis.md)
  * [Apache NiFi vs Memphis](memphis/comparisons/apache-nifi-vs-memphis.md)
* [Privacy Policy](https://memphis.dev/privacy-policy/)

## ⭐ Memphis Schemaverse

* [Overview](memphis-schemaverse/schemaverse-schema-management.md)
* [Getting started](memphis-schemaverse/formats/README.md)
  * [Management](memphis-schemaverse/formats/management.md)
  * [Produce/Consume](memphis-schemaverse/formats/produce-consume/README.md)
    * [Protobuf](memphis-schemaverse/formats/produce-consume/protobuf.md)
    * [JSON Schema](memphis-schemaverse/formats/produce-consume/json-schema.md)
    * [GraphQL](memphis-schemaverse/formats/produce-consume/graphql.md)
    * [Avro](memphis-schemaverse/formats/produce-consume/avro.md)
* [Comparison](memphis-schemaverse/comparison.md)
* [KB](memphis-schemaverse/kb.md)

## ⭐ Memphis Functions

* [Overview](memphis-functions/overview.md)
* [Getting Started](memphis-functions/getting-started.md)

## 📦 Deployment

* [Terraform](deployment/cloud-deployment/README.md)
  * [Deploy on AWS](deployment/cloud-deployment/deploy-on-aws.md)
  * [Deploy on GCP](deployment/cloud-deployment/deploy-on-gcp.md)
  * [Deploy on DigitalOcean](deployment/cloud-deployment/deploy-on-digitalocean.md)
  * [Deploy on Azure](deployment/cloud-deployment/deploy-on-azure.md)
* [Kubernetes](deployment/kubernetes/README.md)
  * [1 - Installation](deployment/kubernetes/1-installation.md)
  * [2 - Access](deployment/kubernetes/2-access.md)
  * [3 - Upgrade](release-notes/how-to-upgrade.md)
* [Docker](deployment/docker-compose.md)
* [Production Best Practices](deployment/production-best-practices.md)

## Client Libraries

* [REST (Webhook)](https://github.com/memphisdev/memphis-http-proxy)
* [Node.js / TypeScript / NestJS](https://github.com/memphisdev/memphis.js)
* [Go](https://github.com/memphisdev/memphis.go)
* [Python](https://github.com/memphisdev/memphis.py)
* [Kotlin (Community)](https://github.com/memphisdev/memphis.kt)
* [.NET](https://github.com/memphisdev/memphis.net)
* [Java](https://github.com/memphisdev/memphis.java)
* [Rust (Community)](https://github.com/turulix/memphis-rust-community)
* [NATS](client-libraries/nats-jetstream.md)
* [Scala](client-libraries/scala.md)

## 🖥 Web Console (GUI)

* [Dashboard](web-console-gui/overview.md)
* [Stations](web-console-gui/stations.md)
* [Users](web-console-gui/users.md)

## 🔌 Integrations Center

* [Index](integrations-center/index.md)
* [Change data Capture (CDC)](integrations-center/change-data-capture-cdc/README.md)
  * [Debezium](integrations-center/change-data-capture-cdc/debezium.md)
* [Monitoring](platform-integrations/monitoring/README.md)
  * [Elasticsearch Observability](platform-integrations/monitoring/elasticsearch-observability.md)
  * [Datadog](platform-integrations/monitoring/datadog.md)
  * [Grafana](platform-integrations/monitoring/grafana.md)
* [Notifications](platform-integrations/notifications/README.md)
  * [Slack](platform-integrations/notifications/slack.md)
* [Storage tiering](platform-integrations/storage/README.md)
  * [S3-Compatible Object Storage](platform-integrations/storage/s3-compatible.md)
* [Source code](integrations-center/source-code/README.md)
  * [GitHub](integrations-center/source-code/github.md)
* [Other platforms](platform-integrations/other-platforms/README.md)
  * [Argo](platform-integrations/other-platforms/argo-and-memphis.md)

## 🗒 Release notes

* [KB](release-notes/kb.md)
* [Releases](release-notes/releases/README.md)
  * [v1.2.0-latest](release-notes/releases/v1.2.0-latest.md)
  * [v1.1.1-stable](release-notes/releases/v1.1.1-stable.md)
  * [v1.1.0](release-notes/releases/v1.1.0-latest.md)
  * [v1.0.3](release-notes/releases/v1.0.3-stable.md)
  * [v1.0.2](release-notes/releases/v1.0.2.md)
  * [v1.0.1](release-notes/releases/v1.0.1.md)
  * [V1.0.0 - GA](release-notes/releases/v1.0.0-lts.md)
  * [v0.4.5 - beta](release-notes/releases/latest-v0.4.5-beta.md)
  * [v0.4.4 - beta](release-notes/releases/v0.4.4-beta.md)
  * [v0.4.3 - beta](release-notes/releases/v0.4.3-beta.md)
  * [v0.4.2 - beta](release-notes/releases/v0.4.2-beta.md)
  * [v0.4.1 - beta](release-notes/releases/v0.4.1-beta.md)
  * [v0.4.0 - beta](release-notes/releases/v0.4.0-beta.md)
  * [v0.3.6 - beta](release-notes/releases/v0.3.6-beta.md)
  * [v0.3.5 - beta](release-notes/releases/v0.3.5-beta.md)
  * [v0.3.0 - beta](release-notes/releases/v0.3.0-beta.md)
  * [v0.2.2 - beta](release-notes/releases/v0.2.2-beta.md)
  * [v0.2.1 - beta](release-notes/releases/v0.2.1-beta.md)
  * [v0.2.0 - beta](release-notes/releases/v0.2.0-beta.md)
  * [v0.1.0 - beta](release-notes/releases/v0.1.0-beta.md)
