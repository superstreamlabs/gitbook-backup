---
description: How to access Memphis after installation
---

# 2 - Access

## Access via UI / CLI / SDK

### For UI

#### <mark style="color:purple;">Localhost</mark>

Run

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

#### <mark style="color:purple;">Production</mark>

Memphis UI uses WebSocket to render components in real time.

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

#### Step 2: Create the LB

```
kubectl expose service memphis-cluster --port=9000,7770  --name=external-service --type=LoadBalancer -n memphis
kubectl -n memphis patch svc external-service -p '{"spec":{"ports": [{"port": 443,"name":"https","targetPort": 9000}]}}'
```

Add the certificate to the LB as well

### For CLI

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

### For SDK

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
