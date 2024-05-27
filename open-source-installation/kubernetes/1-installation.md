---
description: Deploy Memphis over Kubernetes
---

# 1 - Installation

Helm is a K8s package manager that allows users to deploy apps in a single, configurable command. More information about Helm can be found [here](https://helm.sh/docs/topics/charts/).

Memphis is cloud-native and cloud-agnostic to any Kubernetes on **any cloud**.

## Requirements

**Minimum Requirements (Without high availability)**

<table><thead><tr><th>Resource</th><th>Quantity</th><th data-hidden></th></tr></thead><tbody><tr><td>Minimum Kubernetes version</td><td>1.20 and above</td><td></td></tr><tr><td>K8S Nodes</td><td>1</td><td></td></tr><tr><td>CPU</td><td>2 CPU</td><td></td></tr><tr><td>Memory</td><td>4GB RAM</td><td></td></tr><tr><td>Storage</td><td>12GB PVC</td><td></td></tr></tbody></table>

**Recommended Requirements (With high availability)**

| Resource                   | Minimum Quantity  |
| -------------------------- | ----------------- |
| Minimum Kubernetes version | 1.20 and above    |
| K8S Nodes                  | 3                 |
| CPU                        | 4 CPU             |
| Memory                     | 8GB RAM           |
| Storage                    | 12GB PVC Per node |

## Installation

<details>

<summary>Production</summary>

Production-ready Memphis deployment with initial three memphis brokers configured in cluster mode for high availability and higher throughput.

**Stable release**

{% code overflow="wrap" %}
```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install memphis memphis/memphis --set global.cluster.enabled="true" --create-namespace --namespace memphis --wait --version=1.4.4
```
{% endcode %}

**Latest release**

{% code overflow="wrap" %}
```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install --set global.cluster.enabled="true" memphis memphis/memphis --create-namespace --namespace memphis --wait
```
{% endcode %}

</details>

<details>

<summary>Development</summary>

Minimal deployment of Memphis with a single broker

**Stable release**

{% code overflow="wrap" %}
```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install memphis memphis/memphis --create-namespace --namespace memphis --wait --version=1.4.4
```
{% endcode %}

**Latest release**

{% code overflow="wrap" %}
```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install memphis memphis/memphis --create-namespace --namespace memphis --wait
```
{% endcode %}

</details>

Additional helm options can be found [here](1-installation.md#appendix-c-helm-deployment-options).

### Deployed pods

* **memphis.** Memphis broker.
* **memphis-rest-gateway.** Memphis REST Gateway.
* **memphis-metadata.** Metadata store.
* **memphis-metadata-coordinator.** Metadata coordinator

For more information on each component, please head to the [architecture section](../../memphis/architecture.md#key-components).

## Deployment diagram

<figure><img src="../../.gitbook/assets/Memphis Architecture.jpg" alt=""><figcaption></figcaption></figure>

## Appendix A: Install Memphis using predefined parameters

Currently, you can use this for creating users during deployment.

### Execute Helm install with the created values file:

{% code overflow="wrap" %}
```
helm install my-memphis memphis -f config.yaml --create-namespace --namespace memphis --wait
```
{% endcode %}

### Creating users&#x20;

(Based on Memphis password policy: at least 8 characters long, contains both uppercase and lowercase, and at least one number and one special character(!?-@#$%):

{% code title="config.yaml" overflow="wrap" lineNumbers="true" %}
```yaml
auth:
#By default, Memphis sets this option to "false," enabling first user creation during the initial login.
  enabled: true
  users:
    mgmt:
    - user: admin
      password: Admin123456!
    - user: test_mgmt
      password: Test123456!
    - user: test
      password: Test123456@
    client:
    - user: test_app
      password: Test123456!@
    - user: test_app2
      password: Test123456@!
```
{% endcode %}

{% hint style="info" %}
Refer to the example file for guidance: [example/initial\_config\_values.yaml](https://github.com/memphisdev/memphis-k8s/blob/master/examples/initial\_config\_values.yaml)
{% endhint %}

## Appendix B: Dedicated options per specific K8S distributions

{% tabs %}
{% tab title="Red Hat OpenShift" %}
To deploy the Memphis cluster on top of  **Red Hat Openshift** it's necessary to configure default security context parameters as follows:

{% code overflow="wrap" %}
```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis memphis/memphis --set \
global.cluster.enabled="true",\
metadata.postgresql.containerSecurityContext.enabled="false",\
metadata.postgresql.podSecurityContext.enabled="false",\
metadata.pgpool.containerSecurityContext.enabled="false",\
metadata.pgpool.podSecurityContext.enabled="false" \
--create-namespace --namespace memphis --wait
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Appendix C: Helm deployment options

<table><thead><tr><th width="265.3333333333333">Option</th><th>Description</th><th>Default Value</th><th>Example</th></tr></thead><tbody><tr><td>global.cluster.enabled</td><td>Cluster mode for HA and Performance</td><td><code>"false"</code></td><td><code>"false"</code></td></tr><tr><td>exporter.enabled</td><td>Prometheus exporter</td><td><code>"false"</code></td><td><code>"false"</code></td></tr><tr><td>exporter.serviceExposed.enbaled</td><td>Expose metrics port with memphis service</td><td><code>"false"</code></td><td><code>"true"</code></td></tr><tr><td>cluster.enabled</td><td>Enables Memphis cluster deployment. For fully HA configuration use global.cluster.enabled</td><td><code>"false"</code></td><td><code>"true"</code></td></tr><tr><td>cluster.replicas</td><td>Memphis broker replicas</td><td><code>"3"</code></td><td><code>"5"</code></td></tr><tr><td>memphis.image</td><td>Memphis image name</td><td>"memphisos/memphis:x.x.x-stable"</td><td>"memphisos/memphis:latest"</td></tr><tr><td>memphis.ui.port</td><td>Dashboard's (GUI) port</td><td>9000</td><td>9000</td></tr><tr><td>memphis.hosts.uiHostName</td><td>Which URL should be seen as the "UI hostname"</td><td>""</td><td><code>"https://memphis.example.com"</code></td></tr><tr><td>memphis.hosts.restgwHostName</td><td>Which URL should be seen as the "REST Gateway hostname"</td><td>""</td><td><code>"https://restgw.memphis.example.com"</code></td></tr><tr><td>memphis.hosts.brokerHostName</td><td>Which URL should be seen as the "broker hostname"</td><td>""</td><td><code>"memphis.example.com"</code></td></tr><tr><td>memphis.configFile.logsRetentionInDays</td><td>Amount of days to retain system logs</td><td>3</td><td>3</td></tr><tr><td>memphis.configFile.gcProducerConsumerRetentionInHours</td><td>Amount of hours to retain producer/consumer in system</td><td>3</td><td>3</td></tr><tr><td>memphis.configFile.tieredStorageUploadIntervalSeconds</td><td>nterval in seconds between uploads to tiered storage</td><td>8</td><td>8</td></tr><tr><td>memphis.configFile.dlsRetentionHours</td><td>Amount of hours to retain messages in DLS</td><td>3</td><td>3</td></tr><tr><td>memphis.configFile.userPassBasedAuth</td><td>Authentication method selector.<br><code>true = User + pass</code><br><code>false = User + connection token</code></td><td>"true"</td><td>"true"</td></tr><tr><td>memphis.creds.rootPwd</td><td>Root password for the dashboard. Randomly generated.</td><td>""</td><td>"superpass"</td></tr><tr><td>memphis.creds.connectionToken</td><td>Token for connecting an app to the Memphis Message Queue. Auto generated.Randomly generated.</td><td>""</td><td>"connectionToken</td></tr><tr><td>memphis.creds.jwtSecret</td><td>For internal traffic. Randomly generated.</td><td>""</td><td>"&#x3C;JWT_TOKEN>"</td></tr><tr><td>memphis.creds.refreshJwtSecret</td><td>For internal traffic. Randomly generated.</td><td>""</td><td>"&#x3C;JWT_TOKEN>"</td></tr><tr><td>memphis.creds.encryptionSecretKey</td><td>Encryption secret key for internal encryption. Randomly generated.</td><td>""</td><td>""</td></tr><tr><td>memphis.creds.secretConfig.name</td><td>Name of the secret with memphis creds</td><td>"memphis-creds"</td><td>"external-creds"</td></tr><tr><td>memphis.creds.secretConfig.existingSecret</td><td><strong>*Optional*</strong><br>For use of the existing secret with memphis creds</td><td>"false"</td><td>"true"</td></tr><tr><td>memphis.creds.secretConfig.rootPwd_key</td><td><strong>*Optional*</strong><br>Name of the key in secret</td><td>"ROOT_PASSWORD"</td><td>"ROOT_PASSWORD"</td></tr><tr><td>memphis.creds.secretConfig.connectionToken_key</td><td><strong>*Optional*</strong><br>Name of the key in secret</td><td>"CONNECTION_TOKEN"</td><td>"CONNECTION_TOKEN"</td></tr><tr><td>memphis.creds.secretConfig.jwtSecret_key</td><td><strong>*Optional*</strong><br>Name of the key in secret</td><td>"JWT_SECRET"</td><td>"JWT_SECRET"</td></tr><tr><td>memphis.creds.secretConfig.refreshJwtSecret_key</td><td><strong>*Optional*</strong><br>Name of the key in secret</td><td>"REFRESH_JWT_SECRET"</td><td>"REFRESH_JWT_SECRET"</td></tr><tr><td>memphis.creds.secretConfig.encryptionSecretKey_key</td><td><strong>*Optional*</strong><br>Name of the key in secret</td><td>"ENCRYPTION_SECRET_KEY"</td><td>"ENCRYPTION_SECRET_KEY"</td></tr><tr><td>memphis.creds.secretConfig.refreshJwtSecretRestGW_key</td><td><strong>*Optional*</strong><br>Name of the key in secret</td><td>"REFRESH_JWT_SECRET_REST_GW"</td><td>"REFRESH_JWT_SECRET_REST_GW"</td></tr><tr><td>memphis.creds.secretConfig.jwtSecretRestGW_key</td><td><strong>*Optional*</strong><br>Name of the key in secret</td><td>"JWT_SECRET_REST_GW"</td><td>"JWT_SECRET_REST_GW"</td></tr><tr><td>memphis.extraEnvironmentVars.enabled</td><td><strong>*Optional*</strong><br>List of additional environment variables for memphis.</td><td>""</td><td>vars:<br>  - name: KEY<br>  - valye: value</td></tr><tr><td>memphis.tls.verify</td><td><strong>*Optional*</strong><br>For encrypted client-memphis communication. Verification for the CA autority. SSL.</td><td>""</td><td><code>"true"</code></td></tr><tr><td>memphis.tls.secret.name</td><td><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>K8S secret name that holds the certs. SSL.</td><td>""</td><td><code>"memphis-client-tls-secret"</code></td></tr><tr><td>memphis.tls.cert</td><td><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>.pem file to use. SSL.</td><td>""</td><td><code>"memphis_client.pem"</code></td></tr><tr><td>memphis.tls.key</td><td><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>Private key file to use. SSL.</td><td>""</td><td><code>"memphis-key_client.pem"</code></td></tr><tr><td>memphis.tls.ca</td><td><strong>*Optional*</strong><br>For encrypted client-memphis communication.<br>CA file to use. SSL.</td><td>""</td><td><code>"rootCA.pem"</code></td></tr><tr><td>websocket.enabled</td><td>Memphis GUI using websockets for live rendering.</td><td>"true"</td><td>"false"</td></tr><tr><td>websocket.port</td><td>Memphis GUI using websockets for live rendering. The port can be configured</td><td>"7770"</td><td>""</td></tr><tr><td>websocket.host</td><td>Websocket host can be handled on separate LB/DNS. </td><td>"localhost"</td><td>"ws.example.com"</td></tr><tr><td>websocket.noTLS</td><td>Websocket can be configured with tls, default is noTLS.</td><td>"true"</td><td>"false"</td></tr><tr><td>websocket.tls.secret.name</td><td><strong>*Optional*</strong> Memphis GUI using websockets for live rendering.<br>K8S secret name for the certs</td><td>""</td><td>"memphis-ws-tls-secret"</td></tr><tr><td>websocket.tls.cert</td><td><strong>*Optional*</strong><br>Memphis GUI using websockets for live rendering.<br>.pem file to use</td><td>""</td><td>"memphis_local.pem"</td></tr><tr><td>websocket.tls.key</td><td><strong>*Optional*</strong><br>Memphis GUI using websockets for live rendering.<br>key file</td><td>""</td><td>"memphis-key_local.pem"</td></tr><tr><td>metadata.postgresql.username</td><td><strong>*Optional*</strong><br>Username for postgres db</td><td>"postgres"</td><td>"postgres"</td></tr><tr><td>metadata.postgresql.existingSecret</td><td><strong>*Optional*</strong><br>An ability to provide predefined secret for metadata PostgreSQL credentials</td><td>""</td><td>"metadata-creds.yaml"</td></tr><tr><td>metadata.pgpool.existingSecret</td><td><strong>*Optional*</strong><br>An ability to provide predefined secret for metadata PG credentials</td><td>""</td><td>"metadata-creds.yaml"</td></tr><tr><td>metadata.pgpool.tls.enabled</td><td><strong>*Optional*</strong><br>Enabling TLS-based communication with PG</td><td>"false"</td><td>"false"</td></tr><tr><td>metadata.pgpool.tls.certificatesSecret</td><td><strong>*Optional*</strong><br>PG TLS cert secret to be used</td><td>""</td><td>"tls-secret"</td></tr><tr><td>metadata.pgpool.tls.certFilename</td><td><strong>*Optional*</strong><br>PG TLS cert file to be used</td><td>""</td><td>"tls.crt"</td></tr><tr><td>metadata.pgpool.tls.certKeyFilename</td><td><strong>*Optional*</strong><br>PG TLS key to be used</td><td>""</td><td>"tls.key"</td></tr><tr><td>metadata.pgpool.tls.certCAFilename</td><td><strong>*Optional*</strong><br>PG TLS cert CA to be used</td><td>""</td><td>"ca.crt"</td></tr><tr><td>metadata.external.enabled</td><td><strong>*Optional*</strong><br>For using external PG instead of deploying dedicated one for Memphis</td><td>"false"</td><td>"true"</td></tr><tr><td>metadata.external.dbTlsMutual</td><td><strong>*Optional*</strong><br>External PG TLS-basec communication</td><td>"true"</td><td>"true"</td></tr><tr><td>metadata.external.dbName</td><td><strong>*Optional*</strong><br>External PG db name</td><td>""</td><td>"memphis"</td></tr><tr><td>metadata.external.dbHost</td><td><strong>*Optional*</strong><br>External PG db hostname</td><td>""</td><td>"metadata.example.url"</td></tr><tr><td>metadata.external.dbPort</td><td><strong>*Optional*</strong><br>External PG db port</td><td>""</td><td>5432</td></tr><tr><td>metadata.external.dbUser</td><td><strong>*Optional*</strong><br>External PG db user</td><td>""</td><td>"postgres"</td></tr><tr><td>metadata.external.dbPass</td><td><strong>*Optional*</strong><br>External PG db password</td><td>""</td><td>"12345678"</td></tr><tr><td>metadata.external.secret.enabled</td><td><strong>*Optional*</strong><br>Enable an option to use secret for password store</td><td>"false"</td><td>"true"</td></tr><tr><td>metadata.external.secret.name</td><td><strong>*Optional*</strong><br>Secret name</td><td>""</td><td>"metadata-secret"</td></tr><tr><td>metadata.external.secret.dbPass_key</td><td><strong>*Optional*</strong><br>Name of the key in the secret</td><td>""</td><td>"dbPass"</td></tr><tr><td>restGateway.enabled</td><td><strong>*Optional*</strong><br>Memphis Rest Gateway can be disabled if not in use</td><td>"true"</td><td>"false"</td></tr><tr><td>restGateway.jwtSecret</td><td><strong>*Optional*</strong><br>Manual Jwt Token configurtion</td><td>""</td><td>""</td></tr><tr><td>restGateway.refreshJwtSecret</td><td><strong>*Optional*</strong><br>Manual Refresh Jwt Token configurtion</td><td>""</td><td>""</td></tr><tr><td>auth.enabled</td><td><strong>*Optional*</strong><br>Enable using predefined parameters</td><td>"false"</td><td>"true"</td></tr><tr><td>auth.enabled.mgmt</td><td><strong>*Optional*</strong><br>Management users that will be created at first deployment</td><td></td><td></td></tr><tr><td>auth.enabled.client</td><td><strong>*Optional*</strong><br>Client users that will be created at first deployment</td><td></td><td></td></tr></tbody></table>

Search terms: SSL
