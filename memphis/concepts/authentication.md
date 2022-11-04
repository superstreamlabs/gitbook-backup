# Authentication

This document describes authentication and authorisation features in Memphis. Together they allow the operator to control access to the system.

### The Basics

Memphis comprise two types of credentials:

* **Application**.\
  Every connection has an associated user and a connection token which is authenticated.\
  App credentials includes username, connection token, and (optionally) client certificate\* are specified at connection initiation time.\
  There is a **default pair of credentials** called the root user and its detailes will appear on the post-installation notes.
* **Management**.\
  For management purposes only, and to allow control over the UI and CLI, not data, a dedicated user can be created.

[Production environments](https://www.rabbitmq.com/production-checklist.html) should not use the default user and create new user accounts with generated credentials instead.

### Adding a new user

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
