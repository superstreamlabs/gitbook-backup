---
description: How to upgrade Memphis on K8S
---

# 3 - Upgrade

## Best practice procedure for production environments

### Step 1: Deploy the new Memphis server in parallel to the existing one

<figure><img src="../.gitbook/assets/migration #1.jpeg" alt=""><figcaption></figcaption></figure>

### Step 2: Create a second connection for the consumers

Create a second `connection` and `consumer` entities in each existing consumer to the newly created Memphis, so the consumers will consume messages from both the existing Memphis and the newer version.

<figure><img src="../.gitbook/assets/migration #2.jpeg" alt=""><figcaption></figcaption></figure>

### Step 3: Shift the producers to the newer version

Reconnect the producers to produce messages to the newly created Memphis.

<figure><img src="../.gitbook/assets/migration #3.jpeg" alt=""><figcaption></figcaption></figure>

### Step 4: Disconnect the old consumer connections

Once all the existing messages on the older memphis server are read, it is safe to disconnect the older memphis connections and complete the migration.

<figure><img src="../.gitbook/assets/migration #4.jpeg" alt=""><figcaption></figcaption></figure>

## One-to-one replacement: Before v1.0.0 (Not included)

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

```
helm uninstall memphis -n memphis
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

## One-to-one replacement: After v1.0.0 (Included)

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
