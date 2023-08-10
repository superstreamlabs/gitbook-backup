---
cover: ../.gitbook/assets/Memphis Functions (1).jpg
coverY: 0
---

# memphis.yaml

## Parameters table

| Key            | Required / Optional | Description                                                                 | Value type       |
| -------------- | ------------------- | --------------------------------------------------------------------------- | ---------------- |
| function\_name | Required            | Name of the function                                                        | String           |
| language       | Required            | <p>Coding language: <br>Java, Go, PowerShell, Node.js, C#, Python, Ruby</p> | String           |
| description    | Optional            | Function description                                                        | String           |
| tags           | Optional            | Which tags should describe the function                                     | Array of strings |

## File example

{% code title="memphis.yaml" lineNumbers="true" %}
```yaml
function_name: concat keys
description: This function concatenates two keys
tags:
- tag: Production
- tag: strings
language: golang
```
{% endcode %}
