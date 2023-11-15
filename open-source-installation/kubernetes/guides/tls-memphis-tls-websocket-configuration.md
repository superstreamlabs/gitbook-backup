---
description: >-
  In this tutorial, you will discover the steps to enable Memphis's built-in
  Websocket service and generate self-signed certificates to enhance server
  security.
---

# TLS - Memphis TLS websocket configuration

## Create self-signed certificates

Before securing your Memphis deployments, you need to create self-signed certificates. This section provides instructions for generating the necessary certificates.

* Create self-signed certificates for the Memphis server using `mkcert`. These certificates are essential for securing the server:

```bash
$ mkcert \
-cert-file memphis.pem \
-key-file memphis-key.pem  \
"*.memphis.dev"
```

* Next, create a secret resource in the memphis namespace to store these certificates. You can use the following command:

```
kubectl create secret generic tls-secret \
--from-file=memphis.pem --from-file=memphis-key.pem -n memphis
```

* With your self-signed certificates in place, you can deploy Memphis with Websocket support using Helm in a single-line command. Ensure that Websocket communication is enabled and provide the certificate information as follows:

```
helm install my-memphis memphis \
--create-namespace --namespace memphis --wait \
--set \
websocket.tls.cert="memphis.pem",\
websocket.tls.key="memphis-key.pem",\
websocket.tls.secret.name="tls-secret"
```

{% hint style="info" %}
Note: The `global.cluster.enabled` configuration, omitted in this command, is intended for situations in which Memphis is deployed within a cluster environment. Ensure that you activate it as necessary when configuring deployments as a cluster.
{% endhint %}
