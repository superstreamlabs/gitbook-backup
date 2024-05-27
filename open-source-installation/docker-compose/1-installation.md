---
description: Deploy Memphis over Docker using Docker compose
---

# 1 - Installation

## Requirements

| Resource       | Version / Quantity |
| -------------- | ------------------ |
| Docker Engine  | 17.03 and above    |
| Docker compose | v2 and above       |
| CPU            | 1 CPU              |
| Memory         | 4GB                |
| Storage        | 6GB                |

## Getting started

### Step 1: Run one of the following commands

Stable -&#x20;

{% code overflow="wrap" %}
```bash
curl -s https://superstreamlabs.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```
{% endcode %}

Latest -

{% code overflow="wrap" %}
```bash
curl -s https://superstreamlabs.github.io/memphis-docker/docker-compose-latest.yml -o docker-compose-latest.yml && docker compose -f docker-compose-latest.yml -p memphis up
```
{% endcode %}

Output:

```
[+] Running 3/3
 ⠿ Container memphis-memphis-1        Creating                                                      0.2s                                                      0.2s                                                  0.2s
 ⠿ Container memphis-memphis-metadata-1          Creating                                                      0.2s
 ⠿ memphis-memphis-rest-gateway-1                                                  0.2s
```

#### Deployed Containers

* **memphis-1:** The broker itself which acts as the data storage layer. That is the component that stores and controls the ingested messages and their entire lifecycle management.
* **memphis-metadata-1:** Responsible for storing the platform metadata only, such as general information, monitoring, GUI state, and pointers to dead-letter messages. The metadata store uses Postgres.
* **memphis-rest-gateway-1:** Responsible for exposing Memphis management and data ingestion through REST requests.



## Appendix A: Install Memphis using predefined parameters

Currently, you can use this for creating users during deployment.

### Deploy Memphis using the modified docker-compose file:

{% code overflow="wrap" %}
```
docker compose -f docker-compose-dev-with-users.yml -p memphis up
```
{% endcode %}

### Creating users&#x20;

#### Integrate the user list into the docker-compose file within Memphis variables:

(Based on Memphis password policy: at least 8 characters long, contains both uppercase and lowercase, and at least one number and one special character(!?-@#$%):

{% code title="docker-compose-dev-with-users.yml" overflow="wrap" lineNumbers="true" %}
```yaml
    environment:
      ROOT_PASSWORD: memphis
      DOCKER_ENV: true
      ENV: staging
      USER_PASS_BASED_AUTH: true
      CONNECTION_TOKEN: memphis
      METADATA_DB_HOST: memphis-metadata
      INITIAL_CONFIG_FILE: |
          users:
            mgmt:
            - user: admin
              password: Admin123456!
            - user: test_mgmt
              password: Test123456!
            - user: test
              password: Test123456@
            client:
            - user: test_app
              password: Test123456!@
            - user: test_app2
              password: Test123456@!
```
{% endcode %}

{% hint style="info" %}
Refer to the example file for guidance: [example/docker-compose-dev-with-users.yml](https://github.com/memphisdev/memphis-docker/blob/master/examples/docker-compose-dev-with-users.yml)
{% endhint %}
