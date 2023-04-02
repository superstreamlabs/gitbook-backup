---
description: >-
  This section describes authentication and authorization features in Memphis.
  Both methods enable the operator to control access to Memphis.
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Security/Authentication

## The Basics

Memphis has two types of credentials:

* **Application**.\
  Every connection has an associated user and a connection token which is authenticated.\
  App credentials includes username, connection token, and (optionally) client certificate\* are specified at connection initiation time.\
  There is a **default pair of credentials** called the root user and its detailes will appear on the post-installation notes.
* **Management**.\
  For management purposes only, and to allow control over the UI and CLI, not data, a dedicated user can be created.

{% hint style="warning" %}
**Production environments** should not use the default user and create new user accounts with generated credentials instead.
{% endhint %}

## Adding a new user

<details>

<summary>CLI</summary>

1. [Install](../../cli/installation.md) the CLI
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

