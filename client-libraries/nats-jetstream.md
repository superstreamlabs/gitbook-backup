---
description: Memphis compatability with NATS Jetstream
cover: ../../.gitbook/assets/NATS + Memphis.jpeg
coverY: -43.05148658448151
---

# NATS Jetstream

## Introduction

The motivation -

* To enable Memphis users to enjoy the broad reach and integrations of the NATS ecosystem.
* To enable NATS users to make use of Memphis without code changes

## Limitations

* NATS SDKs version - Compatibility with NATS Jetstream 2.9 and above.
* Without Memphis SDK, the following Memphis features will be disabled:
  * Producers/Consumers observability
  * Schemaverse

## Requirements

Memphis operates at the stream level. For a NATS "subject" to be seen and managed by Memphis, it must first be wrapped by a stream.

For example:

#### Connection token based aut:

```markup
nats stream add  -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_APPLICATION_USER>::<MEMPHIS_CONNECTION_TOKEN> 
? Stream Name argo_event_source
? Subjects foo
? Storage file
? Replication 3
? Retention Policy Limits
? Discard Policy Old
? Stream Messages Limit -1
? Per Subject Messages Limit -1
? Total Stream Size -1
? Message TTL -1
? Max Message Size -1
? Duplicate tracking time window 2m0s
? Allow message Roll-ups No
? Allow message deletion Yes
? Allow purging subjects or the entire stream Yes
Stream argo_event_source was created
```

#### User password based auth:

```
nats stream add  -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_APPLICATION_USER> --password=<MEMPHIS_APPLICATION_USER_PASSWORD>
? Stream Name argo_event_source
? Subjects foo
? Storage file
? Replication 3
? Retention Policy Limits
? Discard Policy Old
? Stream Messages Limit -1
? Per Subject Messages Limit -1
? Total Stream Size -1
? Message TTL -1
? Max Message Size -1
? Duplicate tracking time window 2m0s
? Allow message Roll-ups No
? Allow message deletion Yes
? Allow purging subjects or the entire stream Yes
Stream argo_event_source was created
```

## Instructions for specific integrations

{% content-ref url="../../integrations/other-platforms/argo-and-memphis.md" %}
[argo-and-memphis.md](../../integrations/other-platforms/argo-and-memphis.md)
{% endcontent-ref %}
