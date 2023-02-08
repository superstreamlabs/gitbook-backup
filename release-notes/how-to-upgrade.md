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
