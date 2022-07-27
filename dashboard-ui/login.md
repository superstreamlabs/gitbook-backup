# Login

{% hint style="info" %}
For instructions on how to reach the UI, visit the [general section](general.md#how-to-accsess-to-the-ui-after-installation).
{% endhint %}

### The first login will be with the following details:

* **Username:** root
* **Password:** memphis for Docker environments or run the following for k8s environments&#x20;

```
kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode
```

![](<../.gitbook/assets/Screen Shot login>)

{% hint style="info" %}
You can add new users from the [Users page](users.md).
{% endhint %}
