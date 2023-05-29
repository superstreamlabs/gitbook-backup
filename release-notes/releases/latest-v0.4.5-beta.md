---
description: 'Release date: 21 February 2023'
---

# v0.4.5 - beta

Upgrade procedure for production users (Kubernetes deployments)

{% content-ref url="../how-to-upgrade.md" %}
[how-to-upgrade.md](../how-to-upgrade.md)
{% endcontent-ref %}

{% hint style="warning" %}
Please make sure your **SDKs** are **updated** to the latest version to enjoy new features
{% endhint %}

## ![:sparkles:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/2728.png) Added features

* [Tiered storage](broken-reference) - second storage class for out-of-retention messages to enable a better cost-efficient and longer message retention.
* Station name length increased to 128 characters.
* Throughput visualization has improved.
* [Datadog integration](../../integrations/monitoring/datadog.md) has been added to enable external monitoring over Memphis.
* [Grafana integration](../../integrations/monitoring/grafana.md) has been added to enable external monitoring over Memphis.
* `http proxy` Renamed to `rest gateway` to make the comoponent more understandable for new users.
* Ability to produce message without creating an explicit producer object (Available in Go/Python/Node.js SDKs)
* [Memphis configuration](../../memphis/memphis-configuration.md) - ability to configure host names for display purposes
* Broker performance improvements.&#x20;
* Node.js SDK (0.5.1) Go SDK (0.2.1) Python SDK (0.3.2)

## ![:pensive:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/1f614.png) Known issues

* Authentication - Application-type users connect with Memphis (via the SDKs) using a connection token. For now, this token is constant and randomly generated per Memphis deployment.
* While using an older SDK version, an error message appears on the station overview page saying, “Error while getting notified about a poison message: Missing mandatory message headers.”
* When storage capacity reaches 100%, Memphis GUI becomes unresponsive till cleaned up by at least 10%.
* Resending/dropping a great amount of dead-letter messages can significantly take time.
