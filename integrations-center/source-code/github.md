---
description: How to integrate GitHub with Memphis Platform
cover: ../../.gitbook/assets/GitHub + Memphis.jpg
coverY: 0
---

# GitHub

## Introduction

As part of Memphis' developer-first approach and as developers, we recognize that today, a Git repository is much more than code collaboration.&#x20;

It is the main store for code, configuration, schema, scripts, IaC files, and more.

To align with that mindset and to ease the development of Memphis Functions, as well as the integration, and configuration of Memphis itself, Memphis now supports a complete sync with one or more Git repositories for multiple purposes: Memphis Functions, and Memphis Schemaverse.

## Getting started

### 1. Create GitHub access token

1. Go to "`user settings`" in GitHub -> `developer settings` -> `personal access token` -> token -> `generate a new token`.
2. The following permissions are needed per integrated repository:&#x20;
   * _user-only read_ permission.

{% hint style="warning" %}
Please be aware that all keys and credentials are encrypted both in transit and at rest using **AES-256**
{% endhint %}

```
```

![Community Verified icon](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAQAAAAngNWGAAABDUlEQVR4AYXRgUZDYRjH4TegFTKgpEqiFJgoWAoMEQGBgBboChaaAKxLKAhAhQqAdAmpBIQolkCFqp2nITvNKXuA7+/Hhzey5OWjE4Nq3rzY1f9/NGHPB549492+8Ww060iCS2XdctZdI3GsECmb+HJoIX6x6EgDm+lURTH+YB7V9nAqE5WNme4YKuOiY6iMe6PaQxUUIuTbswgFVNJwA8sO3Bn6yR6bWZMSNtJwDtuWfHpQxaPx9C9zadil7jrCigbq6UXceNIVKTWUIqypm2ytJdTiNyNeXclF6GttOVfeDEc7qzjR23r3OMFqZKng1kw0mXGLrfibHTScOZWgGv9TdC6ROFeMTgwYiIxvJzMRWQbeGZUAAAAASUVORK5CYII=)\
