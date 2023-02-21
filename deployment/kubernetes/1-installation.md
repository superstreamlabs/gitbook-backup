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

#### Helm Install Options

| Option                    | Description                                                                                                               | Default Value | Example                       |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------------------------- |
| rootPwd                   | Root password for the dashboard                                                                                           | `"memphis"`   | `"memphis"`                   |
| connectionToken           | Token for connecting an app to the Memphis Message Queue. Auto generated                                                  | `""`          | `"memphis"`                   |
| dashboard.port            | Dashboard's (GUI) port                                                                                                    | 9000          | 9000                          |
| cluster.enabled           | Cluster mode for HA and Performance                                                                                       | `"false"`     | `"false"`                     |
| exporter.enabled          | Prometheus exporter                                                                                                       | `"false"`     | `"false"`                     |
| analytics                 | Collection of anonymous metadata                                                                                          | `"true"`      | `"true"`                      |
| websocket.tls.secret.name | <p><strong>*Optional*</strong> Memphis GUI using websockets for live rendering.<br>K8S secret name for the certs</p>      | ""            | `"memphis-ws-tls-secret"`     |
| websocket.tls.cert        | <p><strong>*Optional*</strong><br>Memphis GUI using websockets for live rendering.<br>.pem file to use</p>                | ""            | `"memphis_local.pem"`         |
| websocket.tls.key         | <p><strong>*Optional*</strong><br>Memphis GUI using websockets for live rendering.<br>key file</p>                        | ""            | `"memphis-key_local.pem"`     |
| memphis.tls.verify        | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication. Verification for the CA autority</p>        | ""            | `"true"`                      |
| memphis.tls.secret.name   | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>K8S secret name that holds the certs</p> | ""            | `"memphis-client-tls-secret"` |
| memphis.tls.cert          | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>.pem file to use</p>                     | ""            | `"memphis_client.pem"`        |
| memphis.tls.key           | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>Private key file to use</p>              | ""            | `"memphis-key_client.pem"`    |
| memphis.tls.ca            | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>CA file to use</p>                       | ""            | `"rootCA.pem"`                |

An example with configured options:

```
helm install memphis --set cluster.replicas=3,rootPwd="rootpassword" memphis/memphis --create-namespace --namespace memphis
```

**Successful deployment** should print the following notes. If not, please raise an issue over [Github](https://github.com/Memphis-OS/memphis-k8s).

```
NAME: memphis
LAST DEPLOYED: Sun Jan  8 20:59:30 2023
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
Memphis UI can be accessed on the following DNS name from within your cluster: memphis-cluster.memphis.svc.cluster.local:9000

To access Memphis using UI/CLI/SDK from localhost, run the below commands:

  - kubectl port-forward service/memphis-cluster 6666:6666 9000:9000 7770:7770 --namespace memphis > /dev/null &

For interacting with the broker via HTTP:

  - kubectl port-forward service/memphis-http-proxy 4444:4444 --namespace memphis > /dev/null &

Dashboard/CLI: http://localhost:9000
Broker: localhost:6666 (Client Connections)
HTTP proxy: localhost:4444 (Data + Mgmt)

---------------------------------------------------------------------------------------------------------------------------------------------
Read more about networking options here: https://docs.memphis.dev/deployment/kubernetes

Website: https://memphis.dev
Documentations: https://docs.memphis.dev

Deployment Information
-------------------------
## Secrets ##
UI/CLI/SDK root username        - root
UI/CLI root Password            - kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode
SDK root connection token       - kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.CONNECTION_TOKEN}" | base64 --decode


## Components ##
Broker - Where data flows through
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



## Deploy Memphis with TLS (encrypted communication)

### 0. Optional: Create self-signed certificates

a) Generate a self-signed certificate using `mkcert`

```bash
$ mkcert -client \
-cert-file memphis_client.pem \
-key-file memphis-key_client.pem  \
"127.0.0.1" "localhost" "*.memphis.dev" ::1 \
email@localhost valera@Valeras-MBP-2.lan
```

b) Find the `rootCA`

```
$ mkcert -CAROOT
```

c) Create self-signed certificates for client

```bash
$ mkcert -client -cert-file client.pem -key-file key-client.pem  localhost ::1 
```

### 1. Create namespace + secret for the TLS certs

a) Create a dedicated namespace for memphis

```bash
kubectl create namespace memphis
```

b) Create a k8s secret with the required certs

{% code overflow="wrap" lineNumbers="true" %}
```bash
kubectl create secret generic memphis-client-tls-secret \
--from-file=memphis_client.pem \
--from-file=memphis-key_client.pem \
--from-file=rootCA.pem -n memphis
```
{% endcode %}

{% code title="memphis-client-tls-secret" lineNumbers="true" %}
```yaml
tls:
  secret:
    name: memphis-client-tls-secret
  ca: "rootCA.pem"
  cert: "memphis_client.pem"
  key: "memphis-key_client.pem"
```
{% endcode %}

### 2. Deploy Memphis with the generated certificate

{% code overflow="wrap" lineNumbers="true" %}
```bash
helm install memphis memphis \
--create-namespace --namespace memphis --wait \
--set \
cluster.enabled="true",\
memphis.tls.verify="true",\
memphis.tls.cert="memphis_client.pem",\
memphis.tls.key="memphis-key_client.pem",\
memphis.tls.secret.name="memphis-client-tls-secret",\
memphis.tls.ca="rootCA.pem"
```
{% endcode %}

## Upgrade existing deployment

### For adding TLS support

1. Create a k8s secret with the provided TLS certs

```
kubectl create secret generic memphis-client-tls-secret \
--from-file=memphis_client.pem \
--from-file=memphis-key_client.pem \
--from-file=rootCA.pem -n memphis
```

2. Upgrade Memphis to use the TLS certs

```bash
helm upgrade memphis memphis -n memphis --reuse-values \
--set \
memphis.tls.verify="true",\
memphis.tls.cert="memphis_client.pem",\
memphis.tls.key="memphis-key_client.pem",\
memphis.tls.secret.name="tls-client-secret",\
memphis.tls.ca="rootCA.pem"
```
