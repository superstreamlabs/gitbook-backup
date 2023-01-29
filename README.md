---
cover: .gitbook/assets/Your_streaming_journey_starts_here..jpg
coverY: 0
---

# Step 1 - Installation

## What is Memphis?

**Memphis** is a next-generation alternative to traditional message brokers.

A simple, robust, and durable cloud-native message broker wrapped with an entire ecosystem that enables cost-effective, fast, and reliable development of modern queue-based use cases.

Memphis enables the building of modern queue-based applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.

**Memphis focuses on four pillars -**

1. Developer Experience - Rapid Development, Modularity, inline processing, Schema management.
2. Observability - Reduces troubleshooting time to near zero.
3. Performance and Efficiency - Provide good performance while maintaining efficient resource consumption.
4. Stability - Queues and brokers play a critical part in the modern application's structure and should be highly available and stable as possible.

## **Getting started**

### **Quick start**

**Kubernetes**

```
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis memphis/memphis --create-namespace --namespace memphis --wait
```

More info can be found in the [Memphis k8s deployment](deployment/kubernetes/) documentation.

**Docker compose (Syntax for v2)**

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```

More info can be found in the [Memphis Docker deployment](deployment/docker-compose.md) documentation.

### How does it work?

<figure><img src=".gitbook/assets/overview (1).jpeg" alt=""><figcaption></figcaption></figure>

## Key features (v0.4.3)

* Fully optimized message broker in under 3 minutes
* Easy-to-use UI, CLI, and SDKs
* Dead-letter station (DLQ)
* Data-level observability
* Runs on your Docker or Kubernetes
* Real-time event tracing
* SDKs: Python, Go, Node.js, Typescript, Nest.JS, Kotlin, .NET, Java
* Embedded schema management using Protobuf, JSON Schema, GraphQL, Avro
* Slack integration

A full roadmap can be found [here](https://memphis.dev/roadmap).
