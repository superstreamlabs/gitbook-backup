# Management

## Create a new Schema

{% tabs %}
{% tab title="GUI" %}
Head to "Schemaverse" on the left menu.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-07-03 at 22.13.50.png" alt=""><figcaption></figcaption></figure>

Click on "Create from blank"

<figure><img src="../../../.gitbook/assets/Screenshot 2023-07-03 at 22.15.04.png" alt=""><figcaption></figcaption></figure>

More information on each format can be found under the [produce/consume](produce-consume/) section.
{% endtab %}

{% tab title="Code" %}
{% code title="Javascript" overflow="wrap" lineNumbers="true" %}
```javascript
await memphisConnection.createSchema({schemaName: "<schema-name>", schemaType: "<schema-type>", schemaFilePath: "<schema-file-path>" });
```
{% endcode %}

{% code title="Python" overflow="wrap" lineNumbers="true" %}
```python
await memphis.create_schema("<schema-name>", "<schema-type>", "<schema-file-path>")
```
{% endcode %}

{% code title="Go" overflow="wrap" lineNumbers="true" %}
```go
err := conn.CreateSchema("<schema-name>", "<schema-type>", "<schema-file-path>")
```
{% endcode %}

{% code title=".NET" overflow="wrap" lineNumbers="true" %}
```aspnet
await client.CreateSchema("<schema-name>", "<schema-type>", "<schema-file-path>")
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Detach a Schema

{% tabs %}
{% tab title="GUI" %}
Head to the designated station

<figure><img src="../../../.gitbook/assets/Screenshot 2023-07-03 at 22.57.55.png" alt=""><figcaption></figcaption></figure>

Click on "Detach" on the upper-center panel

<figure><img src="../../../.gitbook/assets/Screenshot 2023-07-03 at 22.59.39.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Please remember that detaching a schema will enable "any" events to get ingested by the station.
{% endhint %}
{% endtab %}

{% tab title="Code" %}
{% code title="Javascript" overflow="wrap" lineNumbers="true" %}
```javascript
await memphisConnection.detachSchema({ stationName: '<station-name>' });
```
{% endcode %}

{% code title="Python" overflow="wrap" lineNumbers="true" %}
```python
await memphis.detach_schema("<station-name>")
```
{% endcode %}

{% code title="Go" overflow="wrap" lineNumbers="true" %}
```go
err := conn.DetachSchema("<station-name>")
```
{% endcode %}

{% code title=".NET" overflow="wrap" lineNumbers="true" %}
```aspnet
await client.DetachSchema(stationName: station.Name);
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Enforce (attach) a Schema

{% tabs %}
{% tab title="GUI" %}
Head to the designated station.\
On the top-left corner, click on "Enforce schema"

<figure><img src="../../../.gitbook/assets/Screenshot 2023-07-03 at 23.05.21.png" alt=""><figcaption></figcaption></figure>

Choose the required schema.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-07-03 at 23.06.28.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Code" %}
{% code title="Javascript" overflow="wrap" lineNumbers="true" %}
```javascript
await memphisConnection.enforceSchema({ name: '<schema-name>', stationName: '<station-name>' });
```
{% endcode %}

{% code title="Python" overflow="wrap" lineNumbers="true" %}
```python
await memphis.enforce_schema("<schema-name>", "<station-name>")
```
{% endcode %}

{% code title="Go" overflow="wrap" lineNumbers="true" %}
```go
err := conn.EnforceSchema("<schema-name>", "<station-name>")
```
{% endcode %}

{% code title=".NET" overflow="wrap" lineNumbers="true" %}
```aspnet
await client.EnforceSchema(stationName: "<station-name>", schemaName: "<schema-name>");
```
{% endcode %}
{% endtab %}
{% endtabs %}
