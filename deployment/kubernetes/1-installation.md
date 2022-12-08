---
description: Deploy Memphis over Kubernetes
---

# 1 - Installation

{% embed url="https://youtu.be/OmUJXqvFK4M" %}

{% hint style="info" %}
If you prefer using **Terraform**, head [here](../cloud-deployment/)
{% endhint %}

Helm is a k8s package manager that allows users to deploy apps in a single, configurable command.

More information about Helm can be found [here](https://helm.sh/docs/topics/charts/).

Memphis is cloud-native and agnostic to any Kubernetes on **any cloud**.

## Requirements

{% tabs %}
{% tab title="Kubernetes" %}
**Minimum Requirements (No HA)**

<table><thead><tr><th>Resource</th><th>Quantity</th><th data-hidden></th></tr></thead><tbody><tr><td>Minimum Kubernetes version</td><td>1.20 and above</td><td></td></tr><tr><td>K8S Nodes</td><td>1</td><td></td></tr><tr><td>CPU</td><td>2 CPU</td><td></td></tr><tr><td>Memory</td><td>4GB RAM</td><td></td></tr><tr><td>Storage</td><td>12GB PVC</td><td></td></tr></tbody></table>

***

**Recommended Requirements (HA)**

| Resource                   | Minimum Quantity  |
| -------------------------- | ----------------- |
| Minimum Kubernetes version | 1.20 and above    |
| K8S Nodes                  | 3                 |
| CPU                        | 4 CPU             |
| Memory                     | 8GB RAM           |
| Storage                    | 12GB PVC Per node |
{% endtab %}

{% tab title="Docker" %}
**Requirements (No HA)**

| Resource | Quantity              |
| -------- | --------------------- |
| OS       | Mac / Windows / Linux |
| CPU      | 1 CPU                 |
| Memory   | 4GB                   |
| Storage  | 6GB                   |
{% endtab %}
{% endtabs %}

## Installation

<details>

<summary>Production</summary>

Production-grade Memphis with three memphis brokers configured in cluster-mode

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set cluster.enabled="true" memphis/memphis --create-namespace --namespace memphis --wait
```

</details>

<details>

<summary>Dev</summary>

Standard installation of Memphis with a single broker

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis memphis/memphis --create-namespace --namespace memphis --wait
```

</details>

#### Optional: Helm Install Options

| Option          | Description                                                              | Default Value |
| --------------- | ------------------------------------------------------------------------ | ------------- |
| rootPwd         | Root password for the dashboard                                          | `"memphis"`   |
| connectionToken | Token for connecting an app to the Memphis Message Queue. Auto generated | `""`          |
| dashboard.port  | Dashboard's (GUI) port                                                   | 80            |
| cluster.enabled | Cluster mode for HA and Performance                                      | "false"       |
| analytics       | Collection of anonymous metadata                                         | "true"        |

An example with configured options:

```
helm install memphis --set cluster.replicas=1,rootPwd="rootpassword" memphis/memphis --create-namespace --namespace memphis
```

**Successful deployment** should print the following notes. If not, please raise an issue over [Github](https://github.com/Memphis-OS/memphis-k8s).

```
NAME: memphis
LAST DEPLOYED: Thu May  5 17:05:40 2022
NAMESPACE: memphis
STATUS: deployed
REVISION: 1
NOTES:
__  __                      _     _
|  \/  | ___ _ __ ___  _ __ | |__ (_)___
| |\/| |/ _ \ '_ ` _ \| '_ \| '_ \| / __|
| |  | |  __/ | | | | | |_) | | | | \__ \
|_|  |_|\___|_| |_| |_| .__/|_| |_|_|___/
                      |_|

Melvis thank you for installing memphis!
A dev first event-processing platform.

---------------------------------------------------------------------------------------------------------------------------------------------
Memphis UI can be accessed on the following DNS name from within your cluster: memphis-ui.memphis.svc.cluster.local
To access Memphis from localhost, run the below command:
  1. kubectl port-forward service/memphis-cluster 6666:6666 9000:9000 7770:7770 --namespace memphis > /dev/null &

Dashboard: http://localhost:9000
Memphis broker: localhost:6666 (Client/SDK connections)
Memphis broker: localhost:9000 (CLI connection)

---------------------------------------------------------------------------------------------------------------------------------------------
Read more about networking options here: https://docs.memphis.dev/deployment/kubernetes

Website: https://memphis.dev
Documentations: https://docs.memphis.dev

Deployment Information
-------------------------
## Secrets ##
UI/CLI/SDK root username - root
UI/CLI root Password     - kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode

## Components ##
3 Brokers - MQ
UI - GUI Dashboard
MongoDB - Internal Database for management
```

If the output did not save, or regeneration is needed, please use the following command:

```
helm get notes memphis -n memphis
```

#### Deployed Pods

* **memphis-broker-0:** Memphis worker. Responsible for data ingestion and process, just like "Kafka's" brokers
* **memphis-ui-xxx:** UI. Responsible for delivering a graphical user interface for managing the cluster.
* **memphis-mongodb-0/1:** MongoDB, for Memphis internal usage.
