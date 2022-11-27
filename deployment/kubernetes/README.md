---
description: Deploy Memphis over Kubernetes using helm
---

# Kubernetes

{% embed url="https://youtu.be/OmUJXqvFK4M" %}

{% hint style="info" %}

{% endhint %}

### Step 2: Access via UI / CLI / SDK

{% tabs %}
{% tab title="UI" %}

{% endtab %}

{% tab title="CLI" %}
**For the entire CLI reference and how to install it, please head to the following page:**

{% content-ref url="../broken-reference/" %}
[broken-reference](../broken-reference/)
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

{% content-ref url="../broken-reference/" %}
[broken-reference](../broken-reference/)
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

{% content-ref url="../../getting-started/2-hello-world.md" %}
[2-hello-world.md](../../getting-started/2-hello-world.md)
{% endcontent-ref %}
