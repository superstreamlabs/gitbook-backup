---
description: Deploy Memphis over Kubernetes using helm
---

# Kubernetes

{% embed url="https://youtu.be/OmUJXqvFK4M" %}

{% hint style="info" %}
If you prefer using **Terraform**, head [here](cloud-deployment/)
{% endhint %}

Helm is a k8s package manager that allows users to deploy apps in a single, configurable command.

More information about Helm can be found [here](https://helm.sh/docs/topics/charts/).

Memphis is cloud-native and agnostic to any Kubernetes on **any cloud**.

### Requirements

{% tabs %}
{% tab title="Kubernetes" %}
**Minimum Requirements (No HA)**

<table><thead><tr><th>Resource</th><th>Quantity</th><th data-hidden></th></tr></thead><tbody><tr><td>K8S Nodes</td><td>1</td><td></td></tr><tr><td>CPU</td><td>2 CPU</td><td></td></tr><tr><td>Memory</td><td>4GB RAM</td><td></td></tr><tr><td>Storage</td><td>12GB PVC</td><td></td></tr></tbody></table>

***

**Recommended Requirements (HA)**

| Resource  | Minimum Quantity  |
| --------- | ----------------- |
| K8S Nodes | 3                 |
| CPU       | 4 CPU             |
| Memory    | 8GB RAM           |
| Storage   | 12GB PVC Per node |
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

### Step 1: Installation

<details>

<summary>Dev</summary>

Standard installation of Memphis with a single broker

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis memphis/memphis --create-namespace --namespace memphis
```

</details>

<details>

<summary>Production</summary>

Production-grade Memphis with three memphis brokers configured in cluster-mode

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set cluster.enabled="true" memphis/memphis --create-namespace --namespace memphis
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

* **memphis-broker-0:** Three (default) MQ workers (Jetstream- and Memphis-made replicas). Responsible for data ingestion and process, just like "Kafka's" brokers
* **memphis-ui-xxx:** UI. Responsible for delivering a graphical user interface for managing the cluster.
* **memphis-mongodb-0/1:** MongoDB, for Memphis internal usage.

### Step 2: Access via UI / CLI / SDK

{% tabs %}
{% tab title="UI" %}
Expose the UI in a **localhost** environment using "port-forward":

```
kubectl port-forward service/memphis-cluster 6666:6666 9000:9000 7770:7770 --namespace memphis > /dev/null &
```

Credentials

```
UI/CLI root username - root
UI/CLI root Password - kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode
```

[http://localhost:9000](http://localhost:9000)

{% hint style="info" %}
If a simpler localhost connection is needed for more services, use [Kubefwd](https://kubefwd.com/).
{% endhint %}

Expose the UI in a **production** environment:

* Load Balancer

Memphis UI uses WebSocket to render components in real-time.

As WebSockets requires secure communication, it is required to configure SSL cert for the UI to be accessible by a public load balancer and not localhost.

#### Step 1: Deploy self-signed cert

1. Install [mkcert](https://github.com/FiloSottile/mkcert) and generate a certificate

{% code lineNumbers="true" %}
```
mkcert -install
mkcert -cert-file memphis.pem -key-file memphis-key.pem "*.memphis.dev"
```
{% endcode %}

2\. Create a secret with the new cert and private key

```
kubectl create secret generic tls-secret --from-file=memphis.pem --from-file=memphis-key.pem -n memphis
```

3\. Export .conf file and enable tls

```
kubectl -n memphis exec -i memphis-broker-0  -- cat /etc/nats-config/nats.conf | sed  's=no_tls: true=tls {\n    cert_file: "/etc/ssl/certs/memphis.pem"\n    key_file: "/etc/ssl/certs/memphis-key.pem"\n  }=g' > nats.conf
```

4\. Replace the data section with the new tls parameters

```
kubectl create configmap memphis-broker-config --from-file=nats.conf -n memphis --dry-run -o yaml | kubectl replace -f -
```

5\. Add a new volume and volumeMount to the statefulset

```
kubectl edit sts memphis-broker
```

6\. Add the following under the spec.template.spec.containers&#x20;

```
volumeMounts:
  - mountPath: /etc/ssl/certs/memphis.pem
    name: tls-secret
    subPath: memphis_local.pem
  - mountPath: /etc/ssl/certs/memphis-key.pem
    name: tls-secret
    subPath: memphis-key_local.pem
Volumes:
  - name: tls-secret
    secret:
      defaultMode: 420
      secretName: tls-secret
```

7\. Restart Memphis

{% code lineNumbers="true" %}
```
kubectl patch sts memphis-broker  -p '{"spec":{"replicas":0}}' -n  memphis
kubectl patch sts memphis-broker  -p '{"spec":{"replicas":3}}' -n  memphis
```
{% endcode %}

8\. Create the LB

```
kubectl expose service memphis-cluster --port=9000,7770  --name=external-service --type=LoadBalancer -n memphis
kubectl -n memphis patch svc external-service -p '{"spec":{"ports": [{"port": 443,"name":"https","targetPort": 9000}]}}'
```

9\. Add the certificate to the LB as well
{% endtab %}

{% tab title="CLI" %}
**For the entire CLI reference and how to install it, please head to the following page:**

{% content-ref url="broken-reference/" %}
[broken-reference](broken-reference/)
{% endcontent-ref %}

The CLI client connects to Memphis via the UI.

After installing the CLI client and exposing the UI through one of the methods listed under the UI tab, please run the following:

```
$# mem connect

Use command: mem connect --user <user> --password <password> --server <server>

Example: mem connect -u root -p memphis -s <host>:<management_port>
Connection configuration to Memphis
Usage: index connect <command> [options]

Connection to Memphis

Options:
  -u, --user <user>          User
  -p, --password <password>  Password
  -s, --server <server>      Memphis
  -h, --help                 display help for command
```

```
$# mem connect -u root -p memphis -s http://localhost:9000
Connected successfully to Memphis control plane.
```
{% endtab %}

{% tab title="SDK" %}
For more detailed information, please head to the SDKs section below.

{% content-ref url="broken-reference/" %}
[broken-reference](broken-reference/)
{% endcontent-ref %}



**Memphis Node.JS SDK can be used to demonstrate the required parameters.**

```
await memphis.connect({
            host: "<memphis-host>",
            port: <port>, // defaults to 6666
            username: "<username>", // (application type user)
            connectionToken: "<broker-token>", // you will get it on application type user creation
            reconnect: true, // defaults to false
            maxReconnect: 10, // defaults to 10
            reconnectIntervalMs: 1500, // defaults to 1500
            timeoutMs: 1500 // defaults to 1500
});
```

* **host:** Usually "memphis-cluster" or the UI URL.
* **username:** Usually "root". Head to the users' section via the UI or CLI to add more.
* **connectionToken:** Each app that produces and/or consumer data with Memphis uses token authentication. **To display the initial one, please run the following:**

```
kubectl get secret memphis-creds -n <namespace> -o jsonpath="{.data.CONNECTION_TOKEN}" | base64 --decode
```
{% endtab %}
{% endtabs %}

{% content-ref url="../getting-started/2-hello-world.md" %}
[2-hello-world.md](../getting-started/2-hello-world.md)
{% endcontent-ref %}
