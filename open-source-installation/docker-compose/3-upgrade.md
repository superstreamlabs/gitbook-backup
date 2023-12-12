---
description: How to upgrade Memphis on Docker
---

# 3 - Upgrade

## How to upgrade?

### Step 1: shutdown Memphis containers

```bash
docker rm -f $(docker ps -a | grep -i memphis | awk '{print $1}')
```

### Step 2: remove memphis docker images

```bash
docker image rm -f $(docker image ls | grep -i memphis)
```

### Step 3: Reinstall memphis according to the version you upgrade to:

Stable -&#x20;

{% code overflow="wrap" %}
```bash
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```
{% endcode %}

Latest -

{% code overflow="wrap" %}
```bash
curl -s https://memphisdev.github.io/memphis-docker/docker-compose-latest.yml -o docker-compose-latest.yml && docker compose -f docker-compose-latest.yml -p memphis up
```
{% endcode %}
