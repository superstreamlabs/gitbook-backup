---
cover: ../../.gitbook/assets/AWS and Memphis (1).jpeg
coverY: 0
---

# Deploy on AWS

### Introduction

_**Amazon Web Services**_, one of the world's three most popular cloud providers, offers reliable and scalable cloud computing services. Free to join. Pay only for what you use.

At the moment, memphis utilizing [Terraform](https://www.terraform.io/) to automate the entire deployment process from VPC creation, to K8S, to memphis deployment.

Terraform codifies cloud APIs into declarative configuration files.

### Prerequisites





Memphis Deployment on AWS EKS

#### Installation

**Prerequisites**

1. Make sure your machine is connected with [AWS Account](https://portal.aws.amazon.com/billing/signup?nc2=h\_ct\&src=default\&redirect\_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start) using a AWS IAM User which has access to create resources(EKS,VPC,EC2)
2. AWS CLI, [installed](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [configured](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)
3. Terraform is [installed](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/aws-get-started)
4. Kubectl is [installed](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
5. heml is [installed](https://helm.sh/docs/intro/install/)

**Steps**

1. Deploy EKS Cluster using Terraform

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
