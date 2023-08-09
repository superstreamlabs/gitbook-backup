---
description: How to integrate GitHub with Memphis Platform
cover: ../../.gitbook/assets/GitHub + Memphis.jpg
coverY: 0
---

# GitHub

Create GitHub access token

1. Go to settings user in GitHub -> developer settings -> personal access token -> token -> generate new token.
2. Select the following permission to your token: select select repo permission and select user only read permission.

```
Create memphis function
```

<pre><code>In order to define a memphis function:
a. Create in your repository a folder.
b. Create a yaml file in your folder.
c. The yaml file should includes required fields: function_name(string), language(string)
and optional fields: description(string), tags(array of string)

Each function in you repository is a memphis function

Example for yaml file:

<strong>function_name: function name
</strong>description: function description
tags: 
- tag1 
- tag2
- tag3
language: golang
</code></pre>

![Community Verified icon](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAQAAAAngNWGAAABDUlEQVR4AYXRgUZDYRjH4TegFTKgpEqiFJgoWAoMEQGBgBboChaaAKxLKAhAhQqAdAmpBIQolkCFqp2nITvNKXuA7+/Hhzey5OWjE4Nq3rzY1f9/NGHPB549492+8Ww060iCS2XdctZdI3GsECmb+HJoIX6x6EgDm+lURTH+YB7V9nAqE5WNme4YKuOiY6iMe6PaQxUUIuTbswgFVNJwA8sO3Bn6yR6bWZMSNtJwDtuWfHpQxaPx9C9zadil7jrCigbq6UXceNIVKTWUIqypm2ytJdTiNyNeXclF6GttOVfeDEc7qzjR23r3OMFqZKng1kw0mXGLrfibHTScOZWgGv9TdC6ROFeMTgwYiIxvJzMRWQbeGZUAAAAASUVORK5CYII=)\
