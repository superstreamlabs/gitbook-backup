---
description: Deploy Memphis over Docker using Docker compose
---

# Docker Compose

{% embed url="https://youtu.be/cXAk60hMtHs" %}

For easier onboarding and installation, Memphis can be deployed via [docker-compose](https://docs.docker.com/compose/).

### Step 1: Download compose.yaml file

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml
```

### Step 2: Run the compose

```
docker compose -f docker-compose.yml -p memphis up
```

Output:

```
[+] Running 4/4
 ⠿ Container memphis-cluster-1        Creating                                                      0.2s
 ⠿ Container memphis-ui-1             Creating                                                      0.2s
 ⠿ Container memphis-control-plane-1  Creating                                                      0.2s
 ⠿ Container memphis-mongo-1          Creating                                                      0.2s
```

#### Deployed Containers

* **memphis-cluster-1:** Three (default) MQ workers (Jetstream- and Memphis-made replicas). Responsible for data ingestion and processing, just like Kafka's brokers.
* **memphis-ui-1:** UI. Responsible for delivering a graphical user interface for managing the cluster.
* **memphis-control-plane-1:** Control plane. The engine behind Memphis. Responsible for orchestrating the different components and acting as the single source of management.
* **memphis-mongo-1:** MongoDB, for Memphis internal usage.

### Step 3: Access via UI / CLI / SDK

{% tabs %}
{% tab title="UI" %}
The default port of the UI is 9000:

```
http://localhost:9000
```

**Default Username:** root

**Default Password**: memphis
{% endtab %}

{% tab title="CLI" %}
**For the entire CLI reference and how to install it, please head the following page:**

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}



The CLI client connects to Memphis via the UI.

After installing the [CLI client](broken-reference), please run the following:

```
$# mem connect

Use command: mem connect --user <user> --password <password> --server <server>

Example: mem connect -u root -p memphis -s http://<memphis-control-plane>:<port>
Connection configuration to Memphis control plane
Usage: index connect <command> [options]

Connection to Memphis control plane

Options:
  -u, --user <user>          User
  -p, --password <password>  Password
  -s, --server <server>      Memphis control plane
  -h, --help                 display help for command
```



```
$# mem connect -u root -p memphis -s http://localhost:9000
Connected successfully to Memphis control plane.
```
{% endtab %}

{% tab title="SDK" %}
For more detailed information, head to the SDKs section below.

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

####

#### Memphis Node.JS SDK can be used to demonstrate the required parameters.

```
await memphis.connect({
            host: "broker.sandbox.memphis.dev",
            username: "<application type username>",
            connectionToken: "<connection_token>"
});
```

* **host:** Usually the control plane or through the UI URL. For example "https://memphis-ui.test.com/api".
* **username:** Usually "root". Head to the users' section via the UI or CLI to add more.
* **connectionToken:** Each app that produces and/or consumer data with Memphis uses token authentication. <mark style="color:green;">**The default value is "memphis".**</mark>
{% endtab %}
{% endtabs %}
