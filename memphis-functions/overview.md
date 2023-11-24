---
description: Meet Memphis Functions. Write Your Code, And Let the Platform Handle the Rest.
cover: ../.gitbook/assets/Github.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Overview

More and more organizations are leveraging real-time events Serving multiple use cases and a growing number of in-house customers with real-time events than ever before.

Data is constantly evolving and new business requirements continually arise. Making it hard for devs and data teams to keep pace and adapt their pipelines.&#x20;

Here are the usual roadblocks to deploying new event-driven features:

* Implement some SDK and best practices
* Error handling
* Monitoring and logging
* Performance optimizations
* Deployment and integration
* Scale
* HA and FT

And then a new use case arises :(

It's not scalable, no code reuse, it takes time you don't have, and it's hard to debug.

## Meet Memphis Functions

A Dev-first Platform, Created To Equip Any Developer With Data Engineering Superpowers By Developing Or Employing Serverless Functions For Instant Stream Processing.

Memphis Functions works great with other streaming platforms like Kafka, SQS, Pulsar, and others

<figure><img src="../.gitbook/assets/Screenshot 2023-11-24 at 23.50.49.png" alt=""><figcaption></figcaption></figure>

But works perfectly with Memphis Broker

<figure><img src="../.gitbook/assets/Screenshot 2023-11-24 at 23.50.58.png" alt=""><figcaption></figcaption></figure>

### Let's understand a bit more

In simple words, Memphis Functions is another layer within the Memphis platform and "on top" of Memphis Broker.&#x20;

Now, each station has the capability to effortlessly integrate functions for processing and altering events before they reach the consumers. For instance, prior to the availability of Memphis Functions, your station, topic, or queue received nested JSON events requiring consumers to manually flatten them. With Memphis Functions, you have the option to select a pre-built function or create your own to handle this task automatically for every event before consumer groups access them for reading.

### What is so great about it?

It is built for all, super fast to develop, and easy to maintain.

* Multi-language support: Memphis Functions allows you to process events in the most popular programming language of your choice.
* Get to Value Faster: Eliminate the hassle of error handling, dead-letter queues, retries, and offsets. Just write clients using "Functions."
* No Boilerplate, just logic: Say goodbye to crafting wrappers, clients, schemas, and libraries. Let Functions handle that for you.
* Use AI For Writing New Functions: Leverage AI for a rapid development of new processing functions.

### Alpha version

Please bear in mind that Memphis Functions is currently in its Alpha version, so there might be bugs and unexpected behavior that could arise.

Our current focus is directed at enhancing stability, developer experience, and observability. Once these facets are well-established, our attention will pivot towards optimizing performance and dynamism.

Your feedback is critical for its success. Please share it with us through [Discord](https://memphis.dev/discord) or team@memphis.dev
