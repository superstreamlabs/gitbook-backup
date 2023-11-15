# NGINX Ingress Controller and Cloud-Agnostic Memphis Deployments

This page provides an overview of the NGINX Ingress Controller, the deployment process, load balancing concepts, and the idea of "cloud-agnostic" Kubernetes. The tutorial focuses on installing NGINX Ingress for Google Kubernetes Engine (GKE) as a reference.

### NGINX Ingress Controller

**NGINX Ingress** is a popular Kubernetes Ingress controller that manages external access to services in a cluster. It serves as an essential component for routing and managing incoming traffic to your Kubernetes applications.&#x20;

When you deploy NGINX Ingress with the service type `LoadBalancer` , it creates a Network Load Balancer (NLB) in GKE. Here's how this process typically works:

1. **NGINX Ingress Controller**: You have the NGINX Ingress Controller deployed in your GKE cluster. This controller manages and configures NGINX as an Ingress resource within your cluster.
2. **Service with Type LoadBalancer**: When you define a Kubernetes service with the type `LoadBalancer` and configure it as an Ingress controller, this tells GKE to provision a Google Cloud Network Load Balancer.
3. **Network Load Balancer (NLB)**: The GKE LoadBalancer service creates a Google Cloud Network Load Balancer, which is a fully distributed, software-defined, and global load balancer. This NLB routes external traffic to the NGINX Ingress Controller running in your GKE cluster.
4. **Ingress Resources**: You define Kubernetes Ingress resources to specify how incoming HTTP and HTTPS traffic should be routed to specific services and pods within your cluster.

### Introduction to TCP Traffic with NGINX Ingress

NGINX Ingress excels not only in routing HTTP and HTTPS traffic but also efficiently manages TCP traffic. This capability is especially useful when dealing with non-HTTP protocols, such as database connections or custom network protocols.

### NGINX Ingress Deploy Process with helmfile&#x20;

#### Permissions (Required) <a href="#6f98" id="6f98"></a>

* "Cluster Admin" role on your GKE cluster

#### Tools (Required) <a href="#6f98" id="6f98"></a>

* [**Kubernetes client**](https://kubernetes.io/docs/tasks/tools/#kubectl) (`kubectl` command) to interact with Kubernetes
* [**Helm**](https://helm.sh/) (`helm` command) to install Kubernetes packages
* [**helm-diff**](https://github.com/databus23/helm-diff) plugin to see differences in what will be deployed.
* [**helmfile**](https://github.com/helmfile/helmfile) (`helmfile` command) to automate the installation of many helm charts

#### Deployment Steps:

* Create the file below at `helmfile.yaml:`

```
repositories:
  # https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx
  - name: ingress-nginx 
    url: https://kubernetes.github.io/ingress-nginx  

helmDefaults:
  kubeContext: my-kube-context

releases:

  - name: ingress
    installed: true
    namespace: ingress
    chart: ingress-nginx/ingress-nginx
    version: 4.8.3
    values:
      - tcp:
          "7770": "memphis/memphis:7770"
          "6666": "memphis/memphis:6666"
          "4444": "memphis/memphis-rest-gateway:4444"
```

* Deploy the configurations using the following command:

```
helmfile -f helmfile.yaml apply
```

* Here's an example YAML configuration for a simple ingress rule with cert-manager configuration that use internal issuer and configures relevant Memphis ports to be exposed.

```
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-issuer 
  labels: {}
  name: memphis-nginx-ingress
  namespace: memphis
spec:
  rules:
  - host: my-memphis.dev
    http:
      paths:
      - backend:
          service:
            name: memphis
            port:
              number: 9000
        path: /
        pathType: Prefix
  tls:
  - hosts:
    - my-memphis.dev
    secretName: my-tls-certs
EOF
```

{% hint style="info" %}


* `kubernetes.io/ingress.class: nginx` indicates that the NGINX Ingress controller should handle the Ingress.
* `cert-manager.io/cluster-issuer: letsencrypt-prod` specifies the cluster issuer to use for managing TLS certificates. In this case, it's set to "letsencrypt-issuer," which implies that the TLS certificates will be issued by the Let's Encrypt certificate authority for production use.
{% endhint %}

* Create a DNS record that will serve the new ingress service.&#x20;
