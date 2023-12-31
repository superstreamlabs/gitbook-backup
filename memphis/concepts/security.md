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

Memphis has two types of credentials:

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

Soon.
