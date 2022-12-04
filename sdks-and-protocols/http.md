---
description: Memphis HTTP proxy
---

# HTTP

## Deployment

{% hint style="info" %}
First, install Memphis via [K8S](../deployment/kubernetes/) / [Docker](../deployment/docker-compose.md).
{% endhint %}

After Memphis deployment the HTTP proxy will run on port 4444

## Usage

### Authenticate

First you have to authenticate to get JWT token:\
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

### Produce Single Message

{% hint style="info" %}
Attach to every request:\
JWT token as 'Bearer'\
Headers - Content-Type, and any other headers\
Body
{% endhint %}

#### Supported content types:

* text
* application/json
* application/x-protobuf

#### Example:

```
curl --location --request POST 'http_proxy:4444/stations/<station_name>/produce/single' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.4KOGRhUaqvm-qSHnmMwX5VrLKsvHo33u3UdJ0qYP0kI' \
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



### Produce Batch Of Messages&#x20;

{% hint style="info" %}
Attach to every request:\
JWT token as 'Bearer'\
Headers - Content-Type, and any other headers\
Body
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



