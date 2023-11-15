# Scale-out Memphis cluster

### Prerequisites:

Before making any changes to your Memphis cluster, ensure the following prerequisites are met:

1. **Sufficient Worker Nodes:** Each Memphis broker is deployed on a separate node. Verify that you have an adequate number of worker nodes in your Kubernetes cluster.
2. **Stable Network Connectivity:** Ensure network connectivity between the nodes is stable and reliable.
3. **Operational Memphis Cluster:** Verify that your Memphis cluster is in operational mode without issues.

### Step 0:Verify Node and Pod Status

Check the status of your nodes and pods to ensure that they are online and sufficient in number:

```bash
$ kubectl get pods -n memphis
NAME                                            READY   STATUS    RESTARTS      AGE
memphis-0                                       3/3     Running   1 (72s ago)   2m6s
memphis-1                                       3/3     Running   0             2m6s
memphis-2                                       3/3     Running   0             2m6s
memphis-metadata-0                              1/1     Running   0             2m6s
memphis-metadata-1                              1/1     Running   0             2m6s
memphis-metadata-coordinator-79765dcc5c-b8v8t   1/1     Running   0             2m6s
memphis-rest-gateway-6946589d47-b4l24           1/1     Running   0             2m6s
memphis-rest-gateway-6946589d47-qcv7w           1/1     Running   0             2m6s

$ kubectl get nodes
NAME                                                  STATUS   ROLES    AGE    VERSION
gke-gke-testing-4021-gke-testing-4021-5ffa4541-3wr5   Ready    <none>   7d7h   v1.25.12-gke.500
gke-gke-testing-4021-gke-testing-4021-5ffa4541-xz81   Ready    <none>   35s    v1.25.12-gke.500
gke-gke-testing-4021-gke-testing-4021-7495e532-2mz1   Ready    <none>   7d7h   v1.25.12-gke.500
gke-gke-testing-4021-gke-testing-4021-7495e532-9l2n   Ready    <none>   34s    v1.25.12-gke.500
gke-gke-testing-4021-gke-testing-4021-c1aa77b0-d7k2   Ready    <none>   35s    v1.25.12-gke.500
gke-gke-testing-4021-gke-testing-4021-c1aa77b0-j342   Ready    <none>   7d7h   v1.25.12-gke.500
```

### Step 1: Scale Memphis StatefulSet

To increase the number of Memphis replicas, use the following command:

```bash
$ kubectl scale sts memphis --replicas=5 -n memphis
statefulset.apps/memphis scaled
```

### Step 2: Update Memphis Configuration

Edit the `memphis-config` ConfigMap to inform the brokers of the new nodes in the cluster:

* Obtain the `memphis.conf` file:

```
kubectl get cm memphis-config -n memphis -o json | jq -r '.data["memphis.conf"]' > memphis.conf
```

* Edit the `memphis.conf` file to include the additional nodes in the cluster:

{% code overflow="wrap" %}
```yaml
....
###################################
#                                 #
#     Memphis Clustering Setup    #
#                                 #
###################################
cluster {
  port: 6222
  name: memphis

  routes = [
    nats://memphis-0.memphis.memphis.svc.cluster.local:6222,nats://memphis-1.memphis.memphis.svc.cluster.local:6222,nats://memphis-2.memphis.memphis.svc.cluster.local:6222,nats://memphis-3.memphis.memphis.svc.cluster.local:6222,nats://memphis-4.memphis.memphis.svc.cluster.local:6222,

  ]
  cluster_advertise: $CLUSTER_ADVERTISE

  connect_retries: 120
}

debug: false

....
```
{% endcode %}

> Indicate that the brokers **"nats://memphis-3.memphis.memphis.svc.cluster.local:6222" and "nats://memphis-4.memphis.memphis.svc.cluster.local:6222"** have been added to the config file.

### Step 3: Upload the Edited Configuration

Upload the edited configuration to the `memphis-config` ConfigMap:

```basic
kubectl create configmap memphis-config \
  --from-file=memphis.conf \
  -n memphis \
  -o yaml \
  --dry-run=client | kubectl apply -f -
```

### Step 4: Validate the Cluster Health

Check the logs to validate that the cluster is healthy and all the brokers are connected:

```bash
kubectl logs --namespace memphis -l app.kubernetes.io/component=memphis-statefulset
```
