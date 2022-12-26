---
description: Produce messages to a station via HTTP protocol
---

# REST (HTTP)

## Introduction

To enable message production via HTTP calls for various use cases and ease of use, Memphis added an HTTP gateway to receive REST-based requests (=messages) and produce those messages to the required station.

Common use cases that benefit from the REST Gateway are&#x20;

* Produce events directly from a frontend
* Connect Debezium using HTTP Server
* ArgoCD webhooks
* Receive data from Fivetran/Rivery/Any ETL platform using HTTP calls

## Architecture

1. An endpoint creates an HTTP request toward the HTTP Proxy using **port 4444**
2. The HTTP Proxy receives the incoming request and produces it as a message to the station

<figure><img src="../.gitbook/assets/http proxy.jpeg" alt=""><figcaption></figcaption></figure>

For scale requirements, the "HTTP Proxy" component is separate from the brokers' pod and can scale out individually.

## Usage

{% hint style="info" %}
Please make sure your 'http proxy' component is exposed either through localhost or public IP
{% endhint %}

{% hint style="info" %}
The HTTP Proxy URL for the **sandbox** environment is:\
https://proxy.sandbox.memphis.dev
{% endhint %}

### Authenticate

First, you have to authenticate to get a JWT token.\
The JWT token is valid by default for 15 minutes.

#### Example:

```
curl --location --request POST 'http_proxy:4444/auth/authenticate' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "root",
	"connection_token": "memphis"
}'
```

Expected output:&#x20;

```
{"expires_in":900000,"jwt":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.4KOGRhUaqvm-qSHnmMwX5VrLKsvHo33u3UdJ0qYP0kI"}
```

### Refresh Token

Before the JWT token expires, you have to call the refresh token to get a new token, or after authentication failure\
The refresh JWT token is valid by default for 5 hours.

#### Example:

```
curl --location --request POST 'http_proxy:4444/auth/refreshToken' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.RY_LcQ8Halm3Kdk_JxigCN8LDeggKlWwWHiItBK8tVw' \
--data-raw ''
```

Expected output:

```
{"expires_in":900000,"jwt":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.4KOGRhUaqvm-qSHnmMwX5VrLKsvHo33u3UdJ0qYP0kI"}
```

### Produce a single message

{% hint style="info" %}
Attach the JWT token to every request.\
JWT token as '`Bearer`' as a header.
{% endhint %}

#### Supported content types:

* text
* application/json
* application/x-protobuf

#### Example:

```
curl --location --request POST 'http_proxy:4444/stations/<station_name>/produce/single' \
--header 'Authorization: Bearer eyJhbGciOiJIU**********.e30.4KOGRhUaqvm-qSHnmMwX5VrLKsvHo33u3UdJ0qYP0kI' \
--header 'Content-Type: application/json' \
--data-raw '{"message": "New Message"}'
```

Expected output:

```
{"error":null,"success":true}
```

#### Error Example:

```
{"error":"Schema validation has failed: jsonschema: '' does not validate with file:///Users/user/memphisdev/memphis-http-proxy/123#/required: missing properties: 'field1', 'field2', 'field3'","success":false}
```

### Produce a batch of messages&#x20;

{% hint style="info" %}
Attach the JWT token to every request.\
JWT token as '`Bearer`' as a header.
{% endhint %}

#### Supported content types:

* application/json

#### Example:

```
curl --location --request POST 'http_proxy:4444/stations/<station_name>/produce/batch' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.4KOGRhUaqvm-qSHnmMwX5VrLKsvHo33u3UdJ0qYP0kI' \
--header 'Content-Type: application/json' \
--data-raw '[
    {"message": "x"},
    {"message": "y"},
    {"message": "z"}
]'
```

Expected output:

```
{"error":null,"success":true}
```

#### Error Examples:

```
{"errors":["Schema validation has failed: jsonschema: '' does not validate with file:///Users/user/memphisdev/memphis-http-proxy/123#/required: missing properties: 'field1'","Schema validation has failed: jsonschema: '' does not validate with file:///Users/user/memphisdev/memphis-http-proxy/123#/required: missing properties: 'field1'"],"fail":2,"sent":1,"success":false}
```
