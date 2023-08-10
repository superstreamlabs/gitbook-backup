---
description: This section describes how to start operating with Memphis Functions
cover: ../.gitbook/assets/Memphis Functions (1).jpg
coverY: 0
---

# Getting Started

## Create a new custom function

1. Integrate one or more git repositories that contain the functions needed to be synced with Memphis. Options can be found [here](../integrations-center/source-code/).
2. Each new function must reside in its own directory with a dedicated `memphis.yaml`\
   How to write a `memphis.yaml` can be found [here](memphis.yaml.md).
3. Memphis Platform will automatically and periodically parse and read the content of the integrated repositories, and when rules are met and a `memphis.yaml` is found - a function will be presented.
