---
description: >-
  This section details the authentication and authorization functionalities in
  Memphis, both of which allow the operator to manage access control within
  Memphis.
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Security/Authentication

### Type of users

Memphis offers two types of credentials:

* **Application (for clients)**.\
  Each connection is linked with a specific user and either a password or a connection-token for authentication purposes. Application credentials, such as username, password/connection token, and optionally a client certificate, are established at the time of initiating a connection. Additionally, there is a default set of credentials known as the root user, the details of which are provided in the post-installation notes.
* **Management**.\
  Solely for management purposes, this is to facilitate control over the User Interface (UI) and Command Line Interface (CLI).

### Client authentication methods

When deploying Memphis, there is an option to choose the authentication method. The method should be chosen based on the application and the organization's security requirements.

* **Username + password (Default for both self-hosted and cloud).**\
  Each new user (both application and management) gets created with a dedicated username and password.
* **Username + connection token (Deprecated)**\
  Each new application-type user gets created with a dedicated username and a connection-token

### Role-based access control (RBAC)

Role-based access control provides a more detailed level of control, ensuring that particular users can engage with specific stations. This system allows for tailored permissions, such as read-only (consume), write-only (produce), or both read and write access.

RBAC settings can be adjusted via the Web Console during the creation of a new client-type user:

<figure><img src="../../.gitbook/assets/Screenshot 2024-01-01 at 16.03.19.png" alt=""><figcaption></figcaption></figure>

#### How to configure read (consume) permissions

* This can be done either by selecting particular stations or by defining a pattern. For instance, using `prod.*` will include all stations beginning with `prod.` . Additionally, multiple patterns can be specified.

#### How to configure write (produce) permissions

* This can be done either by selecting particular stations or by defining a pattern. For instance, using `prod.*` will include all stations beginning with `prod.` . Additionally, multiple patterns can be specified.
