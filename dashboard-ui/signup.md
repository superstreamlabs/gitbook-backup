# Signup

<figure><img src="../.gitbook/assets/Screen Shot 2022-09-19 at 12.00.10.png" alt=""><figcaption></figcaption></figure>

The reason behind the signup page is to allow the creation of the **first management user** for less technical users that want to quickly login into the system.

### Worth to mention

Memphis is installed with a preconfigured root user that can be used for login into memphis.

**Docker**

Username: `root`

Password: `memphis`

**Kubernetes**

Username: `root`

Password: `kubectl get secret memphis-creds -n memphis -o jsonpath="{.data.ROOT_PASSWORD}" | base64 --decode`
