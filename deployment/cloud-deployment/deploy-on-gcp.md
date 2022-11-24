---
description: Using Terraform
cover: ../../.gitbook/assets/GCP and Memphis.jpeg
coverY: 0
---

# Deploy on GCP

### Introduction

[Google Cloud Platform](https://cloud.google.com/) is one of the world's three most popular cloud providers. It offers a high-performance infrastructure for cloud computing, data analytics & machine learning. Secure, reliable, and high-performance cloud services.

At the moment, memphis utilizing [Terraform](https://www.terraform.io/) to automate the entire deployment process from VPC creation, to K8S, to memphis deployment.

Terraform codifies cloud APIs into declarative configuration files.

### Prerequisites

* A [GCP Account](https://console.cloud.google.com/)
* A [GCP Project](https://console.cloud.google.com/projectcreate) + GCP [Service Account Key](https://cloud.google.com/iam/docs/creating-managing-service-account-keys#iam-service-account-keys-create-console)
* gcloud SDK + CLI [installed](https://cloud.google.com/sdk/docs/quickstarts), configuration depends on station OS.
* Authorize the SDK to access GCP using your user account credentials

```
gcloud auth application-default login
```

* Enable API services:

```
gcloud config set project YOUR_PROJECT_ID
```

```
gcloud services enable compute.googleapis.com container.googleapis.com
```

* [Adjust the "N2\_CPUS"](https://cloud.google.com/docs/quota) quota according to your region. (Default is 8, increase to at least 12)
* Terraform is [installed](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/aws-get-started)
* Kubectl is [installed](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* helm is [installed](https://helm.sh/docs/intro/install/)

### Terraform Installation Flow

<figure><img src="../../.gitbook/assets/gcp memphis terraform.png" alt=""><figcaption></figcaption></figure>

### Step 0: Clone Memphis-Terraform repo

```
git clone git@github.com:memphisdev/memphis-terraform.git && \
cd memphis-terraform/GCP/GKE
```

### Step 1: Deploy GKE Cluster using Terraform

{% hint style="info" %}
**In this step, you will need your `projectID`.**
{% endhint %}

```bash
make infra
```

{% hint style="info" %}
Memphis uses "`makefile`" instead of running three terraform commands
{% endhint %}

### Step 2: Deploy Memphis

```bash
make cluster
```

Once deployment is complete, the Memphis Load Balancer URL will be revealed.

### Step 3: Login to Memphis

Display memphis load balancer public IP by running the following -

```
kubectl get svc -n memphis
```

The UI will be available through **https://\<Public IP>:9000**

### Appendix A: Clean (Remove) Memphis Terraform deployment

Destroy Memphis App -

```bash
make destroymemphis
```

{% hint style="info" %}
**It might take a few minutes for the ALB to be deleted.**
{% endhint %}

Destroy Memphis GKE Cluster -

```bash
make destroyinfra
```
