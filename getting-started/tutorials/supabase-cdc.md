---
description: Produce Supabase CDC events to Memphis using REST Gateway
---

# Supabase CDC

## Introduction

[Supabase](https://supabase.com/) is an open-source Firebase alternative.

In databases, change data capture (or CDC) is a set of software design patterns used to determine and track the data that has changed so that action can be taken using the changed data or because of it.

In simple words: Each time a new event occurs on a specific table, such as INSERT / UPDATE / DELETE, an event will be created and, if configured - published to a destination so some service will be notified for it.

## Flow

1. Install Memphis over Kubernetes
2. Expose Memphis REST Gateway
3. Create a new Supabase table
4. Configure a webhook for the newly created table
5. Check the incoming CDC events in Memphis.

<figure><img src="../../.gitbook/assets/Use cases-Page-8.jpeg" alt=""><figcaption><p>Overview</p></figcaption></figure>

## Step 1: Install Memphis over Kubernetes

