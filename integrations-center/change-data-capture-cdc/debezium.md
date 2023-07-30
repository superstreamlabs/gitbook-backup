---
description: Build Change-Data-Capture pipelines using Debezium and Memphis.dev
cover: ../../.gitbook/assets/Debezium + Memphis.jpeg
coverY: 0
---

# Debezium

## Introduction

[Debezium](https://debezium.io/) is one of the most popular frameworks for collecting Change Data Capture (CDC) events from various databases and can now be easily integrated with Memphis.dev for collecting CDC events from various databases.

In this article, you will learn how to integrate Debezium with Memphis.dev [REST Gateway](https://github.com/memphisdev/memphis-rest-gateway).

{% hint style="info" %}
A full series of articles on "How to build a fully functional CDC pipeline using Postgres, Debezium, and Memphis.dev" can be found [here](https://memphis.dev/blog/part-1-integrating-debezium-server-and-memphis-dev-for-streaming-change-data-capture-cdc-events/).
{% endhint %}

## Getting started

### Step 1: [**Create**](../../web-console-gui/users.md) **a client-type Memphis user to be used by Debezium**

### Step 2: Setup Debezium

Required Debezium configuration (normally stored in the `application.properties` file).

```
debezium.sink.type=http
debezium.sink.http.url=http://<Memphis REST Gateway URL>:4444/stations/todo-cdc-events/produce/single
debezium.sink.http.time-out.ms=500
debezium.sink.http.retries=3
debezium.sink.http.authentication.type=jwt
debezium.sink.http.authentication.jwt.username=<Memphis Application-type username>
debezium.sink.http.authentication.jwt.password=<Memphis Application-type password>
debezium.sink.http.authentication.jwt.url=http://<Memphis REST Gateway URL>:4444/
debezium.format.key=json
debezium.format.value=json
quarkus.log.console.json=false
```

In case Debezium is not installed yet, here is a quick Dockerfile to start one (Don't forget to attach the config file within the container)

```docker
FROM debian:bullseye-slim

RUN apt update && apt upgrade -y && apt install -y openjdk-11-jdk-headless wget git curl && rm -rf /var/cache/apt/*

WORKDIR /
RUN git clone https://github.com/debezium/debezium
WORKDIR /debezium
RUN ./mvnw clean install -DskipITs -DskipTests
WORKDIR /
RUN git clone https://github.com/debezium/debezium-server debezium-server-build
WORKDIR /debezium-server-build
RUN ./mvnw package -DskipITs -DskipTests -Passembly
RUN tar -xzvf debezium-server-dist/target/debezium-server-dist-*.tar.gz -C /
WORKDIR /debezium-server
RUN mkdir data

CMD ./run.sh
```
