# Overview

The dashboard was designed to provide you with a quick snapshot of the cluster health. in one glimpse, you can check if there are any unhealthy stations, determine the stability of the system components, and monitor your system resources.

> All the "Coming Soon" features will be uploaded in subsequent versions.

![](<../.gitbook/assets/Screen Shot 2022-06-15 at 10.51.18.png>)

### Create a [station](stations.md)

To create a station, use the "Create new station" button.

![](<../.gitbook/assets/Create station button (overview)>)&#x20;

![Station details modal](<../.gitbook/assets/create station details (overview)>)

* **Retention** - The default value is seven days. You can choose a custom retention value by time, message size, and message amount.
* ****[**Storage Type**](broken-reference) - Choose whether to store your messages in a file or memory.
* ****[**Replicas**](../memphis/architecture.md#replicas) - Choose how many replicas create behind your station.&#x20;
* **Factory name** - Choose which factory you want to add the new station to.
  * Memphis will create a default factory named "Melvis Factory" if there is no factory.
