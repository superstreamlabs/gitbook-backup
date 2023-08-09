---
description: NATS API Compatibility
cover: ../.gitbook/assets/NATS + Memphis.jpeg
coverY: -43.05148658448151
---

# NATS

{% content-ref url="../" %}
[..](../)
{% endcontent-ref %}

## Introduction

The motivation -

* To enable Memphis users to enjoy the broad reach and integrations of the NATS ecosystem.
* To enable NATS users to make use of Memphis without code changes

## Limitations

* NATS SDKs version - Compatibility with NATS Jetstream 2.9 and above.
* Without Memphis SDK, the following Memphis features will not be supported:
  * Producers/Consumers' observability
  * Schemaverse
  * DLS - resend of anacked messages

## For NATS Jetstream users

Simply change NATS `hostname` to Memphis `hostname`

## For NATS Core users

All of NATS core features will be supported when communicating with Memphis, but without performing the below procedure, Memphis platform will not be able to control those NATS `subjects`.

Memphis operates at the stream level. For a NATS "subject" to be seen and managed by Memphis, it must first be wrapped by a stream.

### Using Memphis Connection token-based authentication:

{% code overflow="wrap" lineNumbers="true" %}
```bash
nats stream add  -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_APPLICATION_USER>::<MEMPHIS_CONNECTION_TOKEN> 
```
{% endcode %}

### Using Memphis password-based authentication:

{% code overflow="wrap" lineNumbers="true" %}
```bash
nats stream add  -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_CLIENT_USER> --password=<MEMPHIS_CLIENT_USER_PASSWORD>
```
{% endcode %}

### (Cloud) Using Memphis password-based authentication (with account ID indication):

{% code overflow="wrap" lineNumbers="true" %}
```bash
nats stream add  -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_CLIENT_USER>$<ACCOUNT_ID> --password=<MEMPHIS_CLIENT_USER_PASSWORD>
```
{% endcode %}

## Allowed characters for stream name

* a-z/A-Z
* 0-9
* \_ -&#x20;

## Combining Nats utils (SDKs/CLI) with Memphis utils (UI/SDKs)

When creating stations using one of the Memphis utils and trying to interact with it using Nats utils the following might happen on the Memphis UI:

* Producer names for messages will reflect as "Unknown"
* Stream name in Nats are case sensitive while in Memphis they are being lower cased so please consider to use only lower cased stream names
* In case the station has been created with Memphis UI/SDK and you want to produce messages into it you will have to send the messages into subject named '\<stream name>$\<partition number(starts from 1)>.final"

## Instructions for specific integrations

{% content-ref url="../platform-integrations/other-platforms/argo-and-memphis.md" %}
[argo-and-memphis.md](../platform-integrations/other-platforms/argo-and-memphis.md)
{% endcontent-ref %}
