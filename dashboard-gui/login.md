# Login

## Initial users

### Docker:

* **Username:** root
* **Password:** memphis

### Kubernetes:

* **Username:** root
* **Password (Run the following to generate):**

```
 kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode
```

{% hint style="info" %}
Additional new users can be added through the [Users page](users.md).
{% endhint %}
