---
description: >-
  This section describes authentication and authorization features in Memphis.
  Both methods enable the operator to control access to Memphis.
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Security/Authentication

## Methods

When deploying Memphis, there is an option to choose the authentication method. The method should be chosen based on the app and the organization's security requirements.

* **Username + password (Default).**\
  Each new user (both application and management) gets created with a dedicated username and password.

Kubernetes deployment command:

```
helm install memphis --set user_pass_based_auth="true" memphis/memphis --create-namespace --namespace memphis
```

Docker deployment command: To change the auth method value, \
please modify the `docker-compose.yml` file

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```

* **Username + connection token**\
  Each new application-type user gets created with a dedicated username but the <mark style="color:red;">same connection token</mark> as the other app-type users.

Kubernetes deployment command:

```
helm install memphis --set user_pass_based_auth="false" memphis/memphis --create-namespace --namespace memphis
```

Docker deployment command: To change the auth method value, \
please modify the `docker-compose.yml` file

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```

## The Basics

Memphis has two types of credentials:

* **Application**.\
  Every connection has an associated user and a password or a connection token that is authenticated.\
  App credentials, including username, password/connection token, and (optionally) client certificate, are specified during connection initiation time.\
  There is a **default pair of credentials** called the root user and its detailes will appear on the post-installation notes.
* **Management**.\
  A dedicated user can be created for management purposes only, and to allow control over the UI and CLI, not data.

{% hint style="warning" %}
**Production environments** should not use the default user and create new user accounts with generated credentials instead.
{% endhint %}

## Adding a new user

<details>

<summary>CLI</summary>

1. Install the CLI
2.  Address the CLI to the cluster&#x20;

    ```powershell
    mem connect -s <memphis broker> -u <root/username> -p <password>
    ```
3.  Create new user

    ```
    mem user add -u yaniv -t application
    ```

    Output -

    ```bash
    User yaniv was created.
    Broker connection credentials: memphis
    These credentials CAN'T be restored, save them in a safe place
    ```

</details>

<details>

<summary>GUI</summary>

1. Head to the "Users" page
2. At the right-top corner, click on "Add a new user"
3. Fill in the required details

</details>

