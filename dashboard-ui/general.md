# General

{% hint style="info" %}
You can access the UI just after the installation of Memphis via K8S / Docker on your dev environment [here](../deployment/kubernetes/).
{% endhint %}

The UI is designed to simplify your work with Memphis and give you a graphical user interface for controlling your stations and observing your data. A few simple clicks can create a messaging queue ready to work, add a function to enrich your data, or monitor/manage your activity.

## Accessing the UI

### **Kubernetes**

*   Expose the UI in a **localhost** environment using "port-forward":

    ```
    $# kubectl port-forward service/memphis-cluster 9000:9000 --namespace memphis & >/dev/null
    ```

    ```
    http://localhost:9000
    ```
*   Expose the UI in a **production** environment

    * Nodeport

    ```
    $# kubectl patch svc memphis-ui --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]'
    ```

    ```
    $# kubectl describe svc memphis-ui | grep -i Endpoints
    Endpoints:                192.150.201.138:80
    ```

    NodePort will expose the UI via one of the k8s workers' IPs

    * Load Balancer

    ```
    $# kubectl patch svc memphis-ui --type='json' -p '[{"op":"replace","path":"/spec/type","value":"LoadBalancer"}]'
    ```

    ```
    $# kubectl describe svc memphis-ui | grep -i "LoadBalancer Ingress"
    LoadBalancer Ingress:     a2d0fd26a0d7941a29d444ac4d03acd3-1181102898.eu-central-1.elb.amazonaws.com
    ```

    * Ingress - Please use the following file:

    ```
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: memphis-ui-ingress
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
                name: memphis-ui
                port:
                  number: 80
    ```

{% hint style="info" %}
More on publishing k8s services [here](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types).
{% endhint %}

### **Docker**

{% hint style="info" %}
For the full docker installation tutorial, please head [here](../deployment/docker-compose.md).
{% endhint %}

The default port of the UI is 9000:

```
http://localhost:9000
```
