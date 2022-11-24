---
description: Deploy Memphis over Kubernetes using Helm
---

# Kubernetes

{% embed url="https://youtu.be/OmUJXqvFK4M" %}

{% hint style="info" %}
If you prefer using **Terraform**, head [here](cloud-deployment/)
{% endhint %}

Helm is a k8s package manager that allows users to deploy apps in a single, configurable command.

More information about Helm can be found [here](https://helm.sh/docs/topics/charts/).

Memphis is cloud-native and agnostic to any Kubernetes on any cloud.  This means it can be deployed over production environments with Helm.

### Requirements

{% tabs %}
{% tab title="Kubernetes" %}
**Minimum Requirements (No HA)**

<table><thead><tr><th>Resource</th><th>Quantity</th><th data-hidden></th></tr></thead><tbody><tr><td>K8S Nodes</td><td>1</td><td></td></tr><tr><td>CPU</td><td>2 CPU</td><td></td></tr><tr><td>Memory</td><td>4GB RAM</td><td></td></tr><tr><td>Storage</td><td>12GB PVC</td><td></td></tr></tbody></table>

****

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

| Resource | Quantity               |
| -------- | ---------------------- |
| OS       | Mac / Windows / Linux  |
| CPU      | 1 CPU                  |
| Memory   | 4GB                    |
| Storage  | 6GB                    |
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

```
helm install <name of the deployment> memphis/memphis --create-namespace --namespace <name os the ns>
```

An example of a command with real values:

```
helm install memphis memphis/memphis --create-namespace --namespace memphis
```

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
  1. kubectl port-forward service/memphis-cluster 6666:6666 9000:9000 --namespace memphis > /dev/null &

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
kubectl port-forward service/memphis-cluster 9000:9000 7770:7770 --namespace memphis > /dev/null &
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

```
kubectl expose service memphis-cluster --port=9000,7770 --name=example-service --type=LoadBalancer -n memphis
```

```
kubectl describe svc memphis-cluster | grep -i "LoadBalancer Ingress"
LoadBalancer Ingress:     a2d0fd26a0d7941a29d444ac4d03acd3-1181102898.eu-central-1.elb.amazonaws.com
```



* Ingress - Please use the following file:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: memphis-cluster-ingress
  namespace: memphis
  annotations:
    acme.cert-manager.io/http01-edit-in-place: "true"
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-staging
spec:
  tls:
  - hosts:
    - "demo.memphis.dev"
    secretName: demo-tls
  rules:
  - host: "demo.memphis.dev"
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: memphis-cluster
            port:
              number: 80
```

{% hint style="info" %}
Find out more on publishing k8s services [here](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types).
{% endhint %}
{% endtab %}

{% tab title="CLI" %}
**For the entire CLI reference and how to install it, please head to the following page:**

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
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

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

####

#### Memphis Node.JS SDK can be used to demonstrate the required parameters.

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











