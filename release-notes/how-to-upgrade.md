---
description: Procedure for upgrading memphis
---

# 3 - Upgrade

### Step 1: Uninstall existing helm installation

```
helm uninstall memphis -n memphis
```

{% hint style="warning" %}
**Data will not be lost!** PVCs are not removed and will be re-attached to the new installation
{% endhint %}

### Step 2: Upgrade Memphis helm repo

```
helm repo update
```

### Step 3: Reinstall Memphis

<details>

<summary>Production</summary>

Production-grade Memphis with three memphis brokers configured in cluster-mode

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis --set cluster.enabled="true" memphis/memphis --create-namespace --namespace memphis --wait
```

</details>

<details>

<summary>Dev</summary>

Standard installation of Memphis with a single broker

```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis memphis/memphis --create-namespace --namespace memphis --wait
```

</details>
