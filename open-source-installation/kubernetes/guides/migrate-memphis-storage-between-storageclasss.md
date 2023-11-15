# Migrate Memphis storage between storageClass's

This tutorial guides you on migrating Persistent Volumes (PVs) from the `gp2` storageClass to the new default `gp3` storageClass. Before implementing this process in production, it is advisable to perform a thorough test in a testing environment.

### Important:

{% hint style="danger" %}
* The migration process includes PVC removal and heavily relies on replication between brokers. Before beginning, validate that there are no errors or warnings.
* To avoid data loss, ensure all critical stations with data are configured with at least 3 replicas.
* Every step should be executed with continuous log monitoring, paying extra attention to the replication process during the broker startup.
{% endhint %}

#### Step 1: Create a new storageClass.yaml file with the required configuration.&#x20;

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp3-new
  annotations:
    storageclass.kubernetes.io/is-default-class: 'true'
provisioner: ebs.csi.aws.com
volumeBindingMode: WaitForFirstConsumer
parameters:
  csi.storage.k8s.io/fstype: xfs
  type: gp3
```

#### Step 2: Apply the New StorageClass Configuration

Begin by applying the configuration for the new storageClass:

```bash
kubectl apply -f storageClass.yaml
```

#### Step 3:Edit the Old StorageClass Configuration

Edit the old storageClass to change it from the default setting. In this example, the old storageClass is named "gp2."

```
kubectl edit storageclasses.storage.k8s.io gp2
```

Make the following change in the configuration:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"storage.k8s.io/v1","kind":"StorageClass","metadata":{"annotations":{"storageclass.kubernetes.io/is-default-class":"true"},"name":"gp2"},"parameters":{"fsType":"ext4","type":"gp2"},"provisioner":"kubernetes.io/aws-ebs","volumeBindingMode":"WaitForFirstConsumer"}
    storageclass.kubernetes.io/is-default-class: "false" ## Change from "true" to "false"
  creationTimestamp: "2022-02-01T12:07:20Z"
  labels:
    k8slens-edit-resource-version: v1
  name: gp2
  resourceVersion: "214718038"
  uid: 5c09a9e5-06a4-4436-bda0-250b279e0475
parameters:
  fsType: ext4
  type: gp2
provisioner: kubernetes.io/aws-ebs
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

#### Step 4: Verify the Configuration

Check the updated storageClass configuration:

```bash
kubectl get storageclasses.storage.k8s.io
NAME               PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
gp2                kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   false                  614d
gp3-ol             kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   true                   328d
gp3-new (default)  ebs.csi.aws.com         Delete          WaitForFirstConsumer   true                   248d
```

#### Step 5: Update PVCs

Delete the PVC (`data-memphis-metadata-0`) associated with the old storageClass. The pod itself will also be deleted, resulting in a new PVC assigned to the new storageClass

{% hint style="danger" %}
#### Ensure the metadata finishes the sync process before going to the next pod.
{% endhint %}

```
$ kubectl get pvc -n memphis
NAME                       STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-memphis-metadata-0    Bound    pvc-b946195d-b1c9-42f1-8541-a650ed37b82d   8Gi        RWO            gp2            36m
data-memphis-metadata-1    Bound    pvc-d30e2baf-a74b-48b4-b1f9-d49c7366501c   8Gi        RWO            gp2            35m
memphis-js-pvc-memphis-0   Bound    pvc-d34787a7-9af2-49c4-b2d2-44418ff9beb2   30Gi       RWO            gp2            31m
memphis-js-pvc-memphis-1   Bound    pvc-888a916e-57a4-4973-bf79-86b924c5e927   30Gi       RWO            gp2            29m
memphis-js-pvc-memphis-2   Bound    pvc-f7bf4f73-69d3-4a5e-89ec-754fd21616ba   30Gi       RWO            gp2            27m
```

Delete resources:

```
$ kubectl delete pvc data-memphis-metadata-0 -n memphis
persistentvolumeclaim "data-memphis-metadata-0" deleted
^C%
$ kubectl delete pod memphis-metadata-0 -n memphis
pod "memphis-metadata-0" deleted
```

Check the status of the PVCs:

```bash
$ kubectl get pvc -n memphis
NAME                       STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-memphis-metadata-0    Bound    pvc-b946195d-b1c9-42f1-8541-a650ed37b82d   8Gi        RWO            gp3-new        36m
data-memphis-metadata-1    Bound    pvc-d30e2baf-a74b-48b4-b1f9-d49c7366501c   8Gi        RWO            gp2            35m
memphis-js-pvc-memphis-0   Bound    pvc-d34787a7-9af2-49c4-b2d2-44418ff9beb2   30Gi       RWO            gp2            31m
memphis-js-pvc-memphis-1   Bound    pvc-888a916e-57a4-4973-bf79-86b924c5e927   30Gi       RWO            gp2            29m
memphis-js-pvc-memphis-2   Bound    pvc-f7bf4f73-69d3-4a5e-89ec-754fd21616ba   30Gi       RWO            gp2            27m
```

#### Step 7: Continue With Other Stateful Resources

Proceed with the migration for the remaining stateful resources. Remember to delete them individually and validate the sync status before each step.

{% hint style="danger" %}
#### Important!!! Deleting and validating stateful resources one by one is crucial for a smooth migration process.
{% endhint %}

