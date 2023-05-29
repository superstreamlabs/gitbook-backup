---
cover: .gitbook/assets/LinkedIn personal (3).png
coverY: 0
---

# Step 1 - Installation

## What is Memphis.dev?

**Memphis.dev** is a next-generation alternative to traditional message brokers.

It enables building modern queue-based applications that require large volumes of streamed and enriched data, modern protocols, zero ops, up to x9 faster development, up to x46 fewer costs, and significantly lower dev time for data-oriented developers and data engineers.

Low footprint, highly resilient, cloud-native, and run on any Kubernetes, on any cloud.

**Memphis focuses on four pillars -**

1. Reliability - Queues and brokers are a mission-critical component in the modern application architecture and should be highly available and stable as possible.
2. Performance and Efficiency - Provide great performance while maintaining efficient resource consumption.
3. Developer Experience - Enable rapid development and ultra-short time-to-production.
4. Observability - Increase observability, integrations with 3rd-party monitoring tools, real-time notifications, stream lineage, and therefore troubleshooting time reduction.

## **Quick Start**

### **Kubernetes**

```
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis memphis/memphis --create-namespace --namespace memphis --wait
```

More info can be found in the [Memphis k8s deployment](deployment/kubernetes/) documentation.

### **Docker compose (Syntax for v2)**

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```

More info can be found in the [Memphis Docker deployment](deployment/docker-compose.md) documentation.

## Walkthrough

{% embed url="https://app.storylane.io/share/upo0paxdvynz" %}

## High-level diagram

<figure><img src=".gitbook/assets/overview (1).jpeg" alt=""><figcaption></figcaption></figure>
