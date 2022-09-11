---
description: Using Terra
cover: ../../.gitbook/assets/AWS and Memphis (1).jpeg
coverY: 0
---

# Deploy on AWS

### Introduction

_**Amazon Web Services**_, one of the world's three most popular cloud providers, offers reliable and scalable cloud computing services. Free to join. Pay only for what you use.

At the moment, memphis utilizing [Terraform](https://www.terraform.io/) to automate the entire deployment process from VPC creation, to K8S, to memphis deployment.

Terraform codifies cloud APIs into declarative configuration files.

### Prerequisites

* [AWS account](https://aws.amazon.com/free/)
* AWS CLI [installed](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [configured](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)
* Make sure your local station is connected with [AWS Account](https://portal.aws.amazon.com/billing/signup?nc2=h\_ct\&src=default\&redirect\_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start) using an AWS IAM user which has access to create resources (EKS, VPC, EC2)

IAM Policy to use -

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "iam:*",
                "kms:*",
                "logs:*",
                "ec2:*",
                "eks:*"
            ],
            "Resource": "*"
        }
    ]
}
```

How to configure AWS CLI -

```bash
$ aws configure
  AWS Access Key ID [****************EF66]: 
  AWS Secret Access Key [****************Fzna]: 
  Default region name [eu-central-1]:
  Default output format [json]:
```

* Terraform is [installed](https://www.terraform.io/downloads)
* Kubectl is [installed](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* helm is [installed](https://helm.sh/docs/intro/install/)

### Terraform Installation Flow

<figure><img src="../../.gitbook/assets/aws memphis terraform (1).png" alt=""><figcaption></figcaption></figure>

### Step 1: Deploy EKS Cluster using Terraform

```bash
make infra
```

1. Deploy Memphis App. Once deployment is complete. You can find Application Load Balancer URL.

```bash
make app
```

**You can view status of load balancer from AWS Account EC2->Load Balancers once its stats is active. You can hit the URL to view Memphis UI**

1. Login Details for root user

```bash
kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode
```

1. Destroy Memphis App.

```bash
make destroyapp
```

**Wait for ALB to be deleted from AWS Console**

1. Destroy Memphis EKS Cluster.

```bash
make destroyinfra
```
