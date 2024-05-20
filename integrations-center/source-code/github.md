---
description: >-
  Please pay attention that Memphis.dev is no longer supported officially by the
  Superstream team (formerly Memphis.dev ) and was released to the public.
cover: ../../.gitbook/assets/GitHub + Memphis.jpg
coverY: 0
---

# GitHub

## Introduction

As part of Memphis' developer-first approach and as developers, we recognize that today, a Git repository is much more than code collaboration.&#x20;

It is the main store for code, configuration, schema, scripts, IaC files, and more.

To align with that mindset and to ease the development of Memphis Functions, as well as the integration, and configuration of Memphis itself, Memphis now supports a complete sync with one or more Git repositories for multiple purposes: Memphis Functions, and Memphis Schemaverse.

## Getting started

### 1. Create a GitHub access token

1. Go to `settings` in GitHub -> `developer settings` -> `personal access token` -> `Fine-grained tokens` -> `generate a new token`.
2. The following permissions are needed per integrated repository:&#x20;
   * Contents: Read-only
3. Copy the generated token into the proper input in the GitHub integration box.

{% hint style="warning" %}
Please be aware that all keys and credentials are encrypted both in transit and at rest using **AES-256**
{% endhint %}

{% @storylane/embed subdomain="app" linkValue="tfefsxzuclc7" url="https://app.storylane.io/share/tfefsxzuclc7" %}
