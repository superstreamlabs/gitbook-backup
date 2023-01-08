---
description: Since v0.4.3, Memphis supports standard NATS SDKs
cover: ../../.gitbook/assets/NATS + Memphis (1).jpeg
coverY: -43.05148658448151
---

# NATS

Introduction

As memphis started from NATS, it was natural to support NATS SDKs as well.&#x20;

The added motivation is -

* To enable Memphis users to enjoy the broad reach and integrations of the NATS ecosystem.
* To enable NATS users to make use of Memphis without code changes

## Limitations

* NATS SDKs version - Compatibility with NATS 2.9 and above.
* Without Memphis SDK, the following Memphis features will be disabled:
  * Producers/Consumers observability
  * Schemaverse
