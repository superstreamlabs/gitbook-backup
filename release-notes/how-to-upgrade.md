---
description: Procedure for upgrading memphis cluster
---

# How to upgrade memphis

{% tabs %}
{% tab title="Docker" %}
Make sure no Memphis container is running

```
docker ps -a
```

Remove containers if needed

```
docker rm -f <container name>
```

Re-run docker-compose installation

{% content-ref url="../deployment/docker-compose.md" %}
[docker-compose.md](../deployment/docker-compose.md)
{% endcontent-ref %}
{% endtab %}

{% tab title="Kubernetes (Helm)" %}
Uninstall existing helm installation

```
helm uninstall my-memphis -n memphis
```

{% hint style="warning" %}
**Data will not be lost!** PVCs are not removed and will re-attach to the new installation
{% endhint %}

Upgrade Memphis helm repo

```
helm repo update
```

Re-install Memphis

{% content-ref url="../deployment/kubernetes.md" %}
[kubernetes.md](../deployment/kubernetes.md)
{% endcontent-ref %}
{% endtab %}
{% endtabs %}

