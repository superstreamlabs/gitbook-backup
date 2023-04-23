---
description: Deploy Memphis over Kubernetes
---

# 1 - Installation

{% hint style="info" %}
If you prefer using **Terraform**, head [here](../cloud-deployment/)
{% endhint %}

Helm is a k8s package manager that allows users to deploy apps in a single, configurable command. More information about Helm can be found [here](https://helm.sh/docs/topics/charts/).

Memphis is cloud-native and cloud-agnostic to any Kubernetes on **any cloud**.

## Requirements

{% tabs %}
{% tab title="Kubernetes" %}
**Minimum Requirements (Without high availability)**

<table><thead><tr><th>Resource</th><th>Quantity</th><th data-hidden></th></tr></thead><tbody><tr><td>Minimum Kubernetes version</td><td>1.20 and above</td><td></td></tr><tr><td>K8S Nodes</td><td>1</td><td></td></tr><tr><td>CPU</td><td>2 CPU</td><td></td></tr><tr><td>Memory</td><td>4GB RAM</td><td></td></tr><tr><td>Storage</td><td>12GB PVC</td><td></td></tr></tbody></table>

***

**Recommended Requirements (With high availability)**

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
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install memphis memphis/memphis --set global.cluster.enabled="true" --create-namespace --namespace memphis --wait
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

#### \* Optional \* Helm deployment options

| Option                               | Description                                                                                                                     | Default Value                            | Example                                  |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| rootPwd                              | Root password for the dashboard                                                                                                 | `"memphis"`                              | `"memphis"`                              |
| user\_pass\_based\_auth              | <p>Authentication method selector.<br><code>true = User + pass</code><br><code>false = User + connection token</code></p>       | `"true"`                                 | `"true"`                                 |
| connectionToken                      | Token for connecting an app to the Memphis Message Queue. Auto generated                                                        | `""`                                     | `"memphis"`                              |
| dashboard.port                       | Dashboard's (GUI) port                                                                                                          | 9000                                     | 9000                                     |
| global.cluster.enabled               | Cluster mode for HA and Performance                                                                                             | `"false"`                                | `"false"`                                |
| exporter.enabled                     | Prometheus exporter                                                                                                             | `"false"`                                | `"false"`                                |
| analytics                            | Collection of anonymous metadata                                                                                                | `"true"`                                 | `"true"`                                 |
| websocket.tls.secret.name            | <p><strong>*Optional*</strong> Memphis GUI using websockets for live rendering.<br>K8S secret name for the certs</p>            | ""                                       | `"memphis-ws-tls-secret"`                |
| websocket.tls.cert                   | <p><strong>*Optional*</strong><br>Memphis GUI using websockets for live rendering.<br>.pem file to use</p>                      | ""                                       | `"memphis_local.pem"`                    |
| websocket.tls.key                    | <p><strong>*Optional*</strong><br>Memphis GUI using websockets for live rendering.<br>key file</p>                              | ""                                       | `"memphis-key_local.pem"`                |
| memphis.tls.verify                   | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication. Verification for the CA autority. SSL.</p>        | ""                                       | `"true"`                                 |
| memphis.tls.secret.name              | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>K8S secret name that holds the certs. SSL.</p> | ""                                       | `"memphis-client-tls-secret"`            |
| memphis.tls.cert                     | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>.pem file to use. SSL.</p>                     | ""                                       | `"memphis_client.pem"`                   |
| memphis.tls.key                      | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>Private key file to use. SSL.</p>              | ""                                       | `"memphis-key_client.pem"`               |
| memphis.tls.ca                       | <p><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>CA file to use. SSL.</p>                       | ""                                       | `"rootCA.pem"`                           |
| metadata.enabled                     |                                                                                                                                 | "true"                                   | "true"                                   |
| metadata.postgresql.image.repository |                                                                                                                                 | "memphisos/memphis-metadata"             | "memphisos/memphis-metadata"             |
| metadata.postgresql.username         |                                                                                                                                 | "postgres"                               | "postgres"                               |
| metadata.fullnameOverride            |                                                                                                                                 | "memphis-metadata"                       | "memphis-metadata"                       |
| metadata.pgpool.image.repository     |                                                                                                                                 | "memphisos/memphis-metadata-coordinator" | "memphisos/memphis-metadata-coordinator" |

Here is how to run an installation command with additional options -&#x20;

```
helm install memphis --set cluster.replicas=3,rootPwd="rootpassword" memphis/memphis --create-namespace --namespace memphis
```

### Deployed pods

* **memphis.** Memphis broker.
* **memphis-rest-gateway.** Memphis REST Gateway.
* **memphis-metadata.** Metadata store.

For more information on each component, please head to the [architecture section](../../memphis/architecture.md#key-components).

## Deploy Memphis with TLS (encrypted communication via SSL)

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
global.cluster.enabled="true",\
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

## Deploy Memphis with an external PostgreSQL instance

### Step 1: Create postgresql\_values.yaml according to the following example:

```
metadata:
  enabled: false
  external:
    enabled: true
    dbTlsMutual: true
    dbName: memphis
    dbHost: <URL>
    dbPort: 5432
    dbUser: postgres
    dbPass: "12345678"
```

### Step 2: Deploy Memphis cluster with external PostgreSQL:

```bash
helm install memphis memphis -f postgresql_values.yaml \
--create-namespace --namespace memphis --wait \
--set \
global.cluster.enabled="true"
```

## Deployment diagram

<figure><img src="../../.gitbook/assets/Memphis Architecture.jpg" alt=""><figcaption></figcaption></figure>

Search terms: SSL
