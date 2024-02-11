---
description: >-
  Memphis is a real-time data processing platform that can be configured to use
  an external metadata database for enhanced scalability and flexibility.
---

# Installing Memphis with an External Metadata Database

#### Prerequisites

* [**Kubernetes client**](https://kubernetes.io/docs/tasks/tools/#kubectl) (`kubectl` command) to interact with Kubernetes.
* [**Helm**](https://helm.sh/) (`helm` command) to install Kubernetes packages.
* An external metadata database ( PostgreSQL) set up and accessible.

#### Deployment Steps - Production Environment:

#### Step 1: Create a Namespace for Memphis deployment

A Kubernetes namespace provides a mechanism to partition cluster resources between multiple users.

```
kubectl create namespace memphis
```

#### Step 2: Create a Secret for the Database Password

When creating a secret for your database password, you must replace `CHANGEIT` with the actual password you use to access your database. This should be done securely to prevent exposing the password:

```
kubectl create secret generic metadata-secret --from-literal=dbPass=CHANGEIT --namespace memphis
```

#### Verifying Creation

After creating the namespace and the secret, you can verify their existence with the following `kubectl` commands:

*   To get memphis namespace:

    ```bash
    kubectl get namespaces memphis
    ```
*   To view the created secret in `memphis`:

    ```bash
    kubectl get secret metadata-secret --namespace memphis
    ```

#### Step 3: Install Memphis Using Helm

Now, you're ready to install Memphis into your Kubernetes cluster, specifically into the `memphis` namespace. Use the `helm install` command:

```
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install memphis memphis/memphis --set \
global.cluster.enabled="true",\
metadata.enabled="false",\
metadata.external.enabled="true",\
metadata.external.dbName=<database_name>,\
metadata.external.dbHost=<database_host>,\
metadata.external.dbPort=<database_port>,\
metadata.external.dbUser=<database_user>,\
metadata.external.secret.enabled="true" \
--create-namespace --namespace memphis --wait
```



#### Deployment Steps - Development Environment:

#### Step 1: Customize Configuration Values

Before running the Helm install command, identify all the `CHANGEIT` placeholder values in your configuration or command instructions.

#### Step 2: Install Memphis Using Helm:

```
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install memphis memphis/memphis --set \
metadata.enabled="false",\
metadata.external.enabled="true",\
metadata.external.dbName=<database_name>,\
metadata.external.dbHost=<database_host>,\
metadata.external.dbPort=<database_port>,\
metadata.external.dbUser=<database_user>,\
metadata.external.dbPass=<database_pass> \
--create-namespace --namespace memphis 
```



After completing the installation of Memphis (or any application) in both production and development environments using Helm, you should verify the installation to ensure that the application's pods are running as expected. The `kubectl get pods --namepsace memphis` command is a straightforward way to check the status of your pods within the memphis namespace.

