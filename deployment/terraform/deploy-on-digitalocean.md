---
cover: ../../.gitbook/assets/DigitalOcean and Memphis.jpeg
coverY: 0
---

# Deploy on DigitalOcean

### Introduction

[DigitalOcean](https://cloud.digitalocean.com/) simplifies cloud computing so builders can spend more time creating software that changes the world.

[DigitalOcean](https://cloud.digitalocean.com/) offers easy-to-use and configure cloud services like servers, kubernetes, object storage, serverless functions, a marketplace of applications, and much more.

[Memphis](../../memphis/overview.md) chose DigitalOcean Kubernetes Marketplace to offer both DO and Memphis users a \
1-click installation of memphis cluster in a production kubernetes environment.

### Prerequisites

* [DigitalOcean](https://cloud.digitalocean.com/) account

### Step 1: Create DigitalOcean Kubernetes Cluster

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-29 at 23.07.31.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-29 at 23.07.45.png" alt=""><figcaption></figcaption></figure>

Nodes are the servers that provide the Kubernetes compute and storage resources

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-29 at 23.08.07.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-29 at 23.08.38.png" alt=""><figcaption></figcaption></figure>

### Step 2: Connect to the Kubernetes cluster

Follow the below instructions to communicate with the newly created cluster

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-29 at 23.11.50.png" alt=""><figcaption></figcaption></figure>

### Step 3: Install Memphis

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-31 at 12.40.59.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-31 at 12.41.04 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2022-08-31 at 12.41.24.png" alt=""><figcaption></figcaption></figure>

We need to create two different load balancers, one for <mark style="color:blue;">**data**</mark> (the broker itself) and one for the <mark style="color:green;">**UI**</mark>.

### Step 4: Expose Memphis for <mark style="color:blue;">data</mark> using a load balancer

Run the following YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    meta.helm.sh/release-name: memphis
    meta.helm.sh/release-namespace: memphis
  creationTimestamp: "2022-07-19T16:25:00Z"
  finalizers:
  - service.kubernetes.io/load-balancer-cleanup
  labels:
    app.kubernetes.io/instance: memphis
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: memphis
    app.kubernetes.io/version: 1.16.0
    helm.sh/chart: memphis-0.1.0
  name: memphis-cluster
  namespace: memphis
spec:
  allocateLoadBalancerNodePorts: true
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - name: memphis-cp-management
    nodePort: 30794
    port: 5555
    protocol: TCP
    targetPort: 5555
  - name: memphis-cp-tcp
    nodePort: 31534
    port: 6666
    protocol: TCP
    targetPort: 6666
  - name: client
    nodePort: 30363
    port: 7766
    protocol: TCP
    targetPort: 7766
  selector:
    app.kubernetes.io/instance: memphis
    app.kubernetes.io/name: memphis
  sessionAffinity: None
  type: LoadBalancer
```

The above will create a digitalocean load balancer with a public ip.

### Step 5: Expose Memphis <mark style="color:green;">UI</mark> using a load balancer

Run the following YAML

```
 kubectl expose deployment memphis-ui --port=80 --target-port=80 \
        --name=memphis-ui --type=LoadBalancer
```

### Step 6: Connect your 1st app

To get the public IPs of the load balancers we created before, run

```
kubectl get svc
```

<figure><img src="../../.gitbook/assets/Screen Shot 2022-09-04 at 23.40.09.png" alt=""><figcaption></figcaption></figure>

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
