---
description: Migrate NATS clients to Memphis
cover: ../.gitbook/assets/NATS + Memphis.jpeg
coverY: -43.05148658448151
---

# NATS

## Introduction

The motivation behind adding compatibility with NATS API is to

* Enable Memphis users to enjoy the broad reach and integrations of the NATS ecosystem.
* Enable a lift & shift type of migration from NATS to Memphis.

## Limitations

* NATS SDKs version - Compatibility with NATS Jetstream 2.9 and above.
* The following Memphis features will not be supported when using NATS SDK:
  * Producers/Consumers' observability
  * Schemaverse
  * Dead-letter station - resend unacked messages

## Replacement process

### For NATS Jetstream clients

<details>

<summary>Cloud</summary>

1. Redirect the `servers` parameter to Memphis Cloud broker `hostname`.\
   It can be found in the main dashboard.

<img src="../.gitbook/assets/Screenshot 2023-09-13 at 15.13.48.png" alt="" data-size="line">

2. In Memphis GUI, create a client-type user based on the one you are (or not) using with NATS and concatenate "$MEMPHIS\_ACCOUNT\_ID" to it.

Code Example (Before)

{% code title="main.py" lineNumbers="true" %}
```python
import asyncio
import nats

async def main():
    connection_opts = {
        "servers": "localhost:4222",
        "allow_reconnect": True,
        "max_reconnect_attempts": 10,
        "reconnect_time_wait": 3,
        "connect_timeout": 15,
        "user":"nats", # Optional in NATS. Mandatory in Memphis.
        "password":"natspassword" # Optional in NATS. Mandatory in Memphis.
    }
    conn = await nats.connect(**connection_opts)

    js = conn.jetstream()
    await js.add_stream(name="test", subjects=["test"])
    await js.publish("test", "hello world".encode())

    await conn.close()

if __name__ == "__main__":
    asyncio.run(main())
```
{% endcode %}

Code Example (After)

{% code title="main.py" lineNumbers="true" %}
```python
import asyncio
import nats

async def main():
    connection_opts = {
        "servers": "aws-eu-central-1.cloud.memphis.dev:6666",
        "allow_reconnect": True,
        "max_reconnect_attempts": 10,
        "reconnect_time_wait": 3,
        "connect_timeout": 15,
        "user":"nats$123456789",
        "password":"natspassword"
    }
    conn = await nats.connect(**connection_opts)

    js = conn.jetstream()
    await js.add_stream(name="test", subjects=["test"])
    await js.publish("test", "hello world".encode())

    await conn.close()

if __name__ == "__main__":
    asyncio.run(main())
```
{% endcode %}

</details>

<details>

<summary>Open-source</summary>

1. Redirect the `servers` parameter to Memphis `hostname`
2. Change port 4222 to 6666
3. In Memphis GUI, create a client-type user based on the one you are (or not) using with NATS

Code Example (Before)

{% code title="main.py" lineNumbers="true" %}
```python
import asyncio
import nats

async def main():
    connection_opts = {
        "servers": "localhost:4222",
        "allow_reconnect": True,
        "max_reconnect_attempts": 10,
        "reconnect_time_wait": 3,
        "connect_timeout": 15,
        "user":"nats", # Optional in NATS. Mandatory in Memphis.
        "password":"natspassword" # Optional in NATS. Mandatory in Memphis.
    }
    conn = await nats.connect(**connection_opts)

    js = conn.jetstream()
    await js.add_stream(name="test", subjects=["test"])
    await js.publish("test", "hello world".encode())

    await conn.close()

if __name__ == "__main__":
    asyncio.run(main())
```
{% endcode %}

Code Example (After)

{% code title="main.py" lineNumbers="true" %}
```python
import asyncio
import nats

async def main():
    connection_opts = {
        "servers": "localhost:6666",
        "allow_reconnect": True,
        "max_reconnect_attempts": 10,
        "reconnect_time_wait": 3,
        "connect_timeout": 15,
        "user":"nats",
        "password":"natspassword"
    }
    conn = await nats.connect(**connection_opts)

    js = conn.jetstream()
    await js.add_stream(name="test", subjects=["test"])
    await js.publish("test", "hello world".encode())

    await conn.close()

if __name__ == "__main__":
    asyncio.run(main())
```
{% endcode %}

</details>

### For NATS Core clients

All of NATS core features will be supported when communicating with Memphis, but without performing the below procedure, the Memphis platform will not be able to control and display the created objects, and therefore it is not recommended.&#x20;

Memphis operates at the stream level. For a NATS `subject` to be visible and managed by Memphis, it must first be wrapped by a `stream`.

<img src="../.gitbook/assets/image (10).png" alt="" data-size="original">

Follow the below instructions based on your Memphis deployment type:

<details>

<summary>Cloud</summary>

1. Install [NATS CLI](https://docs.nats.io/using-nats/nats-tools/nats\_cli).
2. Perform the below instructions.\
   Needed information can be found in the main dashboard.

<img src="../.gitbook/assets/Screenshot 2023-09-13 at 15.13.48.png" alt="" data-size="original">

{% code overflow="wrap" lineNumbers="true" %}
```bash
nats stream add -s <MEMPHIS_BROKER_HOSTNAME>:6666 --user=<MEMPHIS_CLIENT_USER>$<ACCOUNT_ID> --password=<MEMPHIS_CLIENT_USER_PASSWORD>
```
{% endcode %}

</details>

<details>

<summary>Open-source</summary>

1. Install [NATS CLI](https://docs.nats.io/using-nats/nats-tools/nats\_cli).
2. Perform the below instructions based on your Memphis type of authentication:

{% code overflow="wrap" lineNumbers="true" %}
```bash
nats stream add -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_CLIENT_USER> --password=<MEMPHIS_CLIENT_USER_PASSWORD>
```
{% endcode %}

Walkthrough example

{% code overflow="wrap" lineNumbers="true" fullWidth="false" %}
```bash
? Subjects test
? Storage file
? Replication 1
? Retention Policy Limits
? Discard Policy New
? Stream Messages Limit -1
? Per Subject Messages Limit -1
? Total Stream Size -1
? Message TTL -1
? Max Message Size -1
? Duplicate tracking time window 2m0s
? Allow message Roll-ups Yes
? Allow message deletion Yes
? Allow purging subjects or the entire stream Yes
```
{% endcode %}

#### When using Memphis Connection token-based authentication (Legacy OS):

{% code overflow="wrap" lineNumbers="true" %}
```bash
nats stream add -s <MEMPHIS_BROKER_URL>:6666 --user=<MEMPHIS_APPLICATION_USER>::<MEMPHIS_CONNECTION_TOKEN> 
```
{% endcode %}

#### Allowed characters for `stream` name

* a-z/A-Z
* 0-9
* \_ -

Any other character will not be accepted.

</details>

## Important to know

* Messages' producers' names will be displayed as "Unknown".
* `stream` names in NATS are case sensitive, while in Memphis, they are lower-cased, so please consider using only lower-cased names.
* In case a station has been created using Memphis GUI/SDK, and you want to produce messages to the same created station, you will have to send the messages into a subject called `<stream_name>$<partition_number(starts from 1)>.final`.&#x20;
* In case your station name contains a '`.`' sign replace it with '`#`' sign in the subject name level.

## Example

Using Memphis NATS API compatibility to integrate Memphis with Argo

{% content-ref url="../platform-integrations/other-platforms/argo-and-memphis.md" %}
[argo-and-memphis.md](../platform-integrations/other-platforms/argo-and-memphis.md)
{% endcontent-ref %}
