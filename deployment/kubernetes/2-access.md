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
