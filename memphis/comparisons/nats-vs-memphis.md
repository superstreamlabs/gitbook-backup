---
description: This section describes the differeneces between NATS Jetstream and Memphis
---

# NATS Jetstream vs Memphis

## **What is NATS?**

​​NATS is an open-source, message bus technology that powers modern distributed systems. Accordingly, it is responsible for addressing, discovering, and exchanging messages that drive the common patterns in distributed systems, asking and answering questions, aka services/microservices, and making and processing statements, or stream processing.

### Streaming using NATS Jetstream

Firstly, NATS has a built-in distributed persistence system called Jetstream. Specifically, this system enables new functionalities and higher qualities of service on top of the base 'Core NATS' functionalities and qualities of service.\
Jetstream is built-in to nats-server; therefore, you only need 1 (or 3 or 5 if you want fault-tolerance against 1 or 2 simultaneous NATS server failures) of your NATS server(s) to be JetStream enabled for it to be available to all the client applications.

## **What is Memphis.dev?**

After years of operating and researching NATS, Memphis refactored NATS Jetstream to create a fully-fledged event-processing platform for developers.

Memphis.dev enables building next-generation applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.

### **Memphis focuses on four pillars**

1. Stability - Queues and brokers play a critical part in the modern application's structure and should be highly available and stable as possible.
2. Performance and Efficiency - Provide good performance while maintaining efficient resource consumption.
3. Observability - Reduces troubleshooting time to near zero.
4. Developer Experience - Rapid Development, Modularity, inline processing, Schema management.

## Comparison table

| Parameter                          | Memphis                     | Jetstream  |
| ---------------------------------- | --------------------------- | ---------- |
| GUI                                | True                        | False      |
| Schema Management                  | True                        | False      |
| Inline stream enrichment           | True                        | False      |
| Produce/Consume behaviour          | True                        | False      |
| Consumer group                     | True                        | False      |
| Ready-to-use connectors            | True                        | True       |
| Real-time message tracing          | True                        | False      |
| Data-Level Observability           | True                        | False      |
| Automatic environment optimization | True                        | False      |
| Deduplication                      | True. Modified bloom filter | False      |
| Dead-letter                        | True                        | False      |
| REST Gateway                       | True                        | False      |
| Consumer internal communication    | Experimental                | False      |
| Production deployment environment  | Kubernetes                  | Bare-metal |
