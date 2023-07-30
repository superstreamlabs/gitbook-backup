---
description: >-
  On this page, you can see all factory stations with their corresponding
  details
---

# Stations

RabbitMQ has queues, Kafka has topics, and Memphis has stations.

In its simplest form, a station provides a powerful yet easy-to-use messaging queue for apps.

Its true power lies in offloading the business logic from the producers and consumers and embedding it inside the station.

Instead of endless amounts of producers, consumers, orchestrations, manual scaling, and scattered monitoring - just create a station.

<div align="center">

<figure><img src="../.gitbook/assets/Screenshot 2022-12-11 at 14.57.04.png" alt=""><figcaption></figcaption></figure>

</div>

### Create a station

To create a new station, click "Create new station".\
![](<../.gitbook/assets/Screenshot 2022-12-11 at 15.12.31 (1) (1) (1).png>)

A modal will appear with customization options.

<figure><img src="../.gitbook/assets/Screenshot 2022-12-11 at 15.14.03.png" alt=""><figcaption></figcaption></figure>

* **Retention** - The default value is seven days. You can choose a custom retention value by time, message size, and message amount.
* [**Storage type**](broken-reference) - Choose whether to store your messages in a file or memory.
* [**Replicas**](../memphis/architecture.md#replicas) - Choose how many replicas to create behind your station.

### Station overview

All the required information for a specific station is presented here.

<figure><img src="../.gitbook/assets/Screenshot 2022-12-11 at 15.03.54.png" alt=""><figcaption></figcaption></figure>

### Code Example - How to connect an app

Press the SDK button to display the station's connection details.

![](<../.gitbook/assets/Screen Shot 2022-09-19 at 12.14.38.png>)

<div align="left">

<figure><img src="../.gitbook/assets/Screenshot 2022-12-11 at 15.17.27.png" alt=""><figcaption></figcaption></figure>

</div>

* **Language** - The user can change the SDK details per their desired language (for now, Memphis supports Node.js).
* **Installation** - Before the user can use the SDK, they must install Memphis.
* **Code Example -** The example demonstrates how to connect Memphis and create a factory, station, consumer, and producer.

### Producers & Consumers

<figure><img src="../.gitbook/assets/Screenshot 2022-12-11 at 15.05.57.png" alt=""><figcaption></figcaption></figure>

* In this section, Memphis provides data-level observability to the messages within a station
* The right and left panels show lists of producers and consumers.
* The center panel represents the last 100 messages that are currently stored in the station.
