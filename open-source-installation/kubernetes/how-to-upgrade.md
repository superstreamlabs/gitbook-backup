---
description: How to upgrade Memphis on K8S
---

# 3 - Upgrade

Discover step-by-step instructions and best practices for safely and efficiently updating your Memphis installation to the latest version.

### Step 1: Deploy the new Memphis version

<figure><img src="../../.gitbook/assets/migration #1.jpeg" alt=""><figcaption></figcaption></figure>

### Step 2: Create a second connection for the consumers

Establish an additional connection and consumer entities within each existing consumer, enabling them to consume messages from both the existing Memphis and the newly created version.

<figure><img src="../../.gitbook/assets/migration #2.jpeg" alt=""><figcaption></figcaption></figure>

### Step 3: Shift producers to the newer version

Reestablish the connections for the producers to send messages to the newly created Memphis.

<figure><img src="../../.gitbook/assets/migration #3.jpeg" alt=""><figcaption></figcaption></figure>

### Step 4: Disconnect old consumers' connections

After ensuring that all the existing messages on the older Memphis server have been processed, it is secure to disconnect the connections to the older Memphis server and finalize the migration.

<figure><img src="../../.gitbook/assets/migration #4.jpeg" alt=""><figcaption></figcaption></figure>



## 1:1 replacement for >= v1.0.0

### Step 0: Obtain user-supplied values.

```bash
helm get values memphis --namespace memphis
```

### Step 1: Obtain the credentials of your current deployment.

<pre class="language-bash"><code class="lang-bash">export CT=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.CONNECTION_TOKEN}" | base64 -d)
export ROOT_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.ROOT_PASSWORD}" | base64 -d)
<strong>export PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata -o jsonpath="{.data.password}" | base64 -d)
</strong>export REPMGR_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata -o jsonpath="{.data.repmgr-password}" | base64 -d)
export ADMIN_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata-coordinator -o jsonpath="{.data.admin-password}" | base64 -d)
export ENCRYPTION_SECRET_KEY=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.ENCRYPTION_SECRET_KEY}" | base64 -d)
</code></pre>

### Step 2: Uninstall existing helm installation

```
helm uninstall memphis --namespace memphis
```

{% hint style="warning" %}
**Data will not be lost!** PVCs are not removed and will be re-attached to the new installation
{% endhint %}

### Step 3: Upgrade Memphis helm repo

```
helm repo update
```

### Step 4: Reinstall Memphis

<details>

<summary>Production</summary>

Production-grade Memphis with a minimum of three memphis brokers configured in cluster-mode. Add user-supplied values if necessary.

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set global.cluster.enabled="true",metadata.postgresql.password=$PASSWORD,metadata.postgresql.repmgrPassword=$REPMGR_PASSWORD,metadata.pgpool.adminPassword=$ADMIN_PASSWORD,memphis.creds.connectionToken=$CT,memphis.creds.rootPwd=$ROOT_PASSWORD,memphis.creds.encryptionSecretKey=$ENCRYPTION_SECRET_KEY  memphis/memphis --create-namespace --namespace memphis --wait
```

</details>

<details>

<summary>Dev</summary>

Standalone installation of Memphis with a single broker. Add user-supplied values if necessary.

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set metadata.postgresql.password=$PASSWORD,metadata.postgresql.repmgrPassword=$REPMGR_PASSWORD,metadata.pgpool.adminPassword=$ADMIN_PASSWORD,memphis.creds.connectionToken=$CT,memphis.creds.rootPwd=$ROOT_PASSWORD,memphis.creds.encryptionSecretKey=$ENCRYPTION_SECRET_KEY  memphis/memphis --create-namespace --namespace memphis --wait
```

</details>



## Upgrade Memphis cluster with "helm upgrade" using a manual rolling upgrade&#x20;

### Step 0: Obtain user-supplied values. <a href="#step-0-obtain-user-supplied-values." id="step-0-obtain-user-supplied-values."></a>

```
helm get values memphis --namespace memphis
```

### Step 1: Obtain the credentials of your current deployment <a href="#step-1-obtain-the-credentials-of-your-current-deployment" id="step-1-obtain-the-credentials-of-your-current-deployment"></a>

```bash
export CT=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.CONNECTION_TOKEN}" | base64 -d)
export ROOT_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.ROOT_PASSWORD}" | base64 -d)
export PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata -o jsonpath="{.data.password}" | base64 -d)
export REPMGR_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata -o jsonpath="{.data.repmgr-password}" | base64 -d)
export ADMIN_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-metadata-coordinator -o jsonpath="{.data.admin-password}" | base64 -d)
export ENCRYPTION_SECRET_KEY=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.ENCRYPTION_SECRET_KEY}" | base64 -d)
```

### Step 2: Delete the statefulset with cascade=orphan option <a href="#step-2-delete-the-statefulset-with-cascade-orphan-option" id="step-2-delete-the-statefulset-with-cascade-orphan-option"></a>

```bash
kubectl delete statefulset memphis --cascade=orphan -n memphis
```

### Step 3: Run helm upgrade with all the values you need + updateStrategy=OnDelete <a href="#step-3-run-helm-upgrade-with-all-the-values-you-need-+-updatestrategy-ondelete" id="step-3-run-helm-upgrade-with-all-the-values-you-need-+-updatestrategy-ondelete"></a>

```
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update &&helm upgrade --install memphis --set metadata.postgresql.password=$PASSWORD,metadata.postgresql.repmgrPassword=$REPMGR_PASSWORD,metadata.pgpool.adminPassword=$ADMIN_PASSWORD,memphis.creds.connectionToken=$CT,memphis.creds.rootPwd=$ROOT_PASSWORD,memphis.creds.encryptionSecretKey=$ENCRYPTION_SECRET_KEY memphis/memphis --create-namespace --namespace memphis --wait
```

### Step 4: Upgrade brokers. Delete one by one and validate each one to get back to the online state. <a href="#step-4-upgrade-brokers.-delete-one-by-one-and-validate-each-one-to-get-back-to-the-online-state." id="step-4-upgrade-brokers.-delete-one-by-one-and-validate-each-one-to-get-back-to-the-online-state."></a>

## 1:1 replacement for versions < v1.0.0

### Step 0: Obtain user-supplied values.

```bash
helm get values memphis --namespace memphis
```

### Step 1: Obtain the credentials of your current deployment.

```bash
export CT=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.CONNECTION_TOKEN}" | base64 -d)
export ROOT_PASSWORD=$(kubectl get secret --namespace "memphis" memphis-creds -o jsonpath="{.data.ROOT_PASSWORD}" | base64 -d)
```

### Step 2: Uninstall existing helm installation

```bash
helm uninstall memphis -n memphis
```

{% hint style="warning" %}
**Data will not be lost!** PVCs are not removed and will be re-attached to the new installation
{% endhint %}

### Step 3: Upgrade Memphis helm repo

```bash
helm repo update
```

### Step 4: Reinstall Memphis

<details>

<summary>Production</summary>

Production-grade Memphis with a minimum of three memphis brokers configured in cluster-mode. Add user-supplied values if necessary.

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set global.cluster.enabled="true",connectionToken=$CT,rootPwd=$ROOT_PASSWORD memphis/memphis --create-namespace --namespace memphis --wait
```

</details>

<details>

<summary>Dev</summary>

Standalone installation of Memphis with a single broker. Add user-supplied values if necessary.

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set connectionToken=$CT,rootPwd=$ROOT_PASSWORD memphis/memphis --create-namespace --namespace memphis --wait
```

</details>
