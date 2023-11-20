---
description: >-
  Transitioning Memphis.dev from a self-hosted setup to the cloud: A
  step-by-step guide.
---

# Self-hosted to Cloud

This manual will guide you through the process of migrating your Memphis.dev self-hosted deployment to Memphis.dev Cloud, a serverless service designed to simplify and enhance the scalability and reliability of your Memphis.dev deployment, as well as your capabilities.

## High-level plan

1. **Introduction**
   * Why Migrate to Memphis.dev Cloud?
   * Prerequisites
2. **Migration Steps**
   * Step 1: Assess Your Open-Source Deployment Requirements
   * Step 2: Configure your Memphis.dev Cloud Accordingly
   * Step 3: Configure your Clients
   * Step 4: Migrate Data and Clients

### 1. Introduction

#### Why Migrate to Memphis.dev Cloud?

Memphis.dev Cloud offers several advantages over self-hosted Memphis.dev deployments. These benefits include:

* **Seamless Scalability:** Experience limitless growth potential with Memphis.dev Cloud. Easily scale to meet your evolving needs without the hassle of managing hardware.
* **Enhanced Performance:** Enjoy lightning-fast response times and superior performance with our optimized cloud infrastructure.
* **Automatic Updates:** Stay at the forefront of Memphis.dev with automatic updates and enhancements. Our cloud version ensures you're always running the latest and greatest version of Memphis.
* **Effortless Maintenance:** Leave server maintenance and monitoring to us. Focus on what matters most, while we handle the backend tasks, ensuring your nervous system is always up and running.
* **Data Security:** Protect your valuable data with our top-notch cloud security measures. Benefit from data encryption, regular backups, and industry-standard security protocols.
* **Global Accessibility:** Access your data from anywhere in the world. Memphis.dev cloud ensures that your data is accessible whenever and wherever your apps need it.

#### Prerequisites

Before you start the migration process, ensure you have the following prerequisites in place:

1. Access to your existing Memphis.dev self-hosted deployment
2. Memphis.dev Cloud account ([Sign up](https://cloud.memphis.dev) if you haven't already)
3. Make sure to have the same users (both client and management) on both the self-hosted deployment and Memphis.dev Cloud

### 2. **Migration Steps**

#### Step 1: Assess Your Open-Source Deployment Requirements

In essence, there are no features exclusive to the self-hosted version that are absent in the cloud; however, there are cloud-specific features that are not available in the self-hosted, meaning you will not have any missing capabilities. That being said, the cloud offers various plans, ranging from free to enterprise, each unlocking different features and scalability options. Please make sure to subscribe to the plan that fits your needs.

A full plans and features comparison can be found on the [pricing page](https://memphis.dev/pricing).

#### Step 2: Configure your Memphis.dev Cloud Accordingly

Please make sure the following are configured:

* Users
* Storage tiering (if needed)
* Integrations (if needed)
* Stations: automatically generated when producers or consumers are created, following their default configurations. If specific values other than the defaults are required, they must be explicitly coded or established through Memphis.dev Cloud Console.

#### Step 3: Configure your Clients

When transitioning a self-hosted client to Memphis.dev Cloud, no code modifications are required. Instead, you'll need to include an extra connection parameter known as the '`account id`'

Self-hosted client's connection block -

```javascript
memphis.connect({
            host: MEMPHIS_HOST,
            username: MEMPHIS_USERNAME,
            password: MEMPHIS_PASSWORD,
});
```

Cloud client's connection block -

```javascript
memphis.connect({
            host: MEMPHIS_HOST,
            username: MEMPHIS_USERNAME,
            password: MEMPHIS_PASSWORD,
            accountId: 123456789
});
```

Clients can be adjusted prior to the migration process and still function with the additional parameter in the self-hosted deployment, as this parameter will be seamlessly ignored.

#### Step 4: Migrate Data and Clients

1. By the time you reach step 4, you should find yourself in a comparable situation: a fully configured and operational Memphis.dev Cloud alongside your active self-hosted deployment.\
   <img src="../.gitbook/assets/image (11).png" alt="" data-size="original">
2. Create a second connection and consumer entities in each existing consumer to the newly created Memphis, so the consumers will consume messages from both the existing Memphis and the newer version.\
   ![](<../.gitbook/assets/image (12).png>)
3. Reconnect the producers to produce messages to the newly created Memphis.\
   ![](<../.gitbook/assets/image (13).png>)
4. Once all the existing messages on the older memphis server are read, it is safe to disconnect the older memphis connections and complete the migration.\
   ![](<../.gitbook/assets/image (14).png>)
