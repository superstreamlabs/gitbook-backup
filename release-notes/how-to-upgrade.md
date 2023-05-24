---
description: Procedure for upgrading memphis
---

# 3 - Upgrade

## Below v1.0.0 (Not included)

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

## Above v1.0.0 (Included)

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
helm install memphis --set global.cluster.enabled="true",metadata.postgresql.password=$PASSWORD,metadata.postgresql.repmgrPassword=$REPMGR_PASSWORD,metadata.pgpool.adminPassword=$ADMIN_PASSWORD,connectionToken=$CT,rootPwd=$ROOT_PASSWORD  memphis/memphis --create-namespace --namespace memphis --wait
```

</details>

<details>

<summary>Dev</summary>

Standalone installation of Memphis with a single broker. Add user-supplied values if necessary.

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set metadata.postgresql.password=$PASSWORD,metadata.postgresql.repmgrPassword=$REPMGR_PASSWORD,metadata.pgpool.adminPassword=$ADMIN_PASSWORD,connectionToken=$CT,rootPwd=$ROOT_PASSWORD  memphis/memphis --create-namespace --namespace memphis --wait
```

</details>
