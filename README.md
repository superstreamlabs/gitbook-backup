# 1 - Installation

### What is Memphis{dev}?

Memphis{dev} **is** the only low-code messaging platform that provides a full ecosystem **for** in-app streaming use cases using memphis distributed message broker with a paradigm of produce-consume **that** supports modern in-app streaming pipelines and async communication **by** removing frictions of management, cost, resources, language barriers, and time **for** data-oriented developers and data engineers, **unlike** other message brokers and queues that requires a great amount of code, optimizations, adjustments, and mainly time.

[memphis.dev](https://memphis.dev/) started as a fork of the [nats.io](http://nats.io/) project (since 2011), written in GoLang, and creating its own stream on top. Instead of topics and queues, Memphis uses stations, which will become an entity with embedded logic in the future.

**Memphis focuses on four pillars**

1. Performance - Enhancing cache usage
2. Resiliency - Never lose a message and 99.99995% uptime
3. Observability - Out-of-the-box observability that makes sense and reduces troubleshooting time
4. Developer Experience - Modularity, inline processing, schema management, gitops ability

**Choose your preferred environment -**&#x20;

* ****[Kubernetes](deployment/kubernetes.md)
* [Docker-Compose](deployment/docker-compose.md#step-1-download-compose.yaml-file)

{% hint style="info" %}
If you prefer not to install - Sandbox [https://sandbox.memphis.dev](https://sandbox.memphis.dev)
{% endhint %}

[For more information on Memphis.](memphis/overview.md)
