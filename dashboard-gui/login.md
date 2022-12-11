# Login

{% hint style="info" %}
For instructions on how to reach the UI, visit the [general section](general.md#how-to-accsess-to-the-ui-after-installation).
{% endhint %}

### Docker:

* **Username:** root
* **Password:** memphis

### Kubernetes:

* **Username:** root
* **Password (Command to generate):** `kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode`

![](<../.gitbook/assets/Screenshot 2022-12-11 at 14.50.52.png>)

{% hint style="info" %}
You can add new users from the [Users page](users.md).
{% endhint %}
