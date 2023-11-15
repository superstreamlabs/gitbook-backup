# Expanding Memphis Disk Storage

Kubernetes is commonly employed to deploy stateful applications like Memphis within your cluster. Each Pod in the [StatefulSets](https://www.howtogeek.com/834810/what-are-kubernetes-statefulsets-when-should-you-use-them) retains access to its [local persistent volumes](https://kubernetes.io/blog/2019/04/04/kubernetes-1.14-local-persistent-volumes-ga) even after rescheduling, ensuring the individual state is maintained, separate from neighboring Pods within the set.

However, these volumes have a notable limitation: Kubernetes does not offer a straightforward way to resize them through the StatefulSet object. The `spec.resources.requests.storage` property within the `volumeClaimTemplates` field of the StatefulSet [is immutable](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset) and restricts you from making necessary capacity increases. This article will demonstrate how to work around this limitation.

### Step 1: Verify Dynamic Resize Availability

Start by confirming if dynamic resizing is available in the storage class. If not, add the following line to enable it:

```yaml
allowVolumeExpansion: true
```

### Step 2: Increase PVC Size

To increase the size of the attached PVC (repeat for all PVC instances), execute the following command:

```bash
kubectl patch pvc  memphis-js-pvc-memphis-0 -p '{"spec":{"resources":{"requests":{"storage":"600Gi"}}}}' -n memphis
```

### Step 3: Update StatefulSet Configuration

Export the Memphis StatefulSet configuration to a YAML file and adjust the size of the PVC accordingly:

```bash
k get sts memphis -n memphis -o yaml > sts.yaml
```

Edit the file to reflect the new PVC size within the `spec` section:

```yaml
    spec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 600Gi
      volumeMode: Filesystem
```

### Step 4: Modify "updateStrategy"

Change the "updateStrategy" type to "OnDelete" within the `sts.yaml` file:

```yaml
  updateStrategy:
    type: OnDelete
```

### Step 5: Update memphis-config ConfigMap

Update the `memphis-config` ConfigMap with the new size for the `storageEngine` space. Note that the reload feature will not work, so you will need to restart all the brokers one by one manually. Modify the `storageEngine` section as follows:

```yaml
storageEngine {
  max_mem: 8Gi
  store_dir: /data

  max_file:600Gi
}
```

### Step 6: Delete the Current StatefulSet with the Orphan Option

Execute the following command to delete the current StatefulSet, ensuring the use of the `--cascade=orphan` option:

```bash
kubectl delete statefulset --cascade=orphan memphis -n memphis
```

### Step 7: Deploy Edited StatefulSet

Deploy the edited StatefulSet to the cluster using the `sts.yaml` file:

```
kubectl apply -f sts.yaml -n memphis
```

### Step 8: Delete and Validate Pod Updates

Delete the Pods individually and confirm that they have the new configuration and retain the previous data.

{% hint style="danger" %}
#### Important!!! Deleting and validating stateful resources one by one is crucial for a smooth migration process.
{% endhint %}
