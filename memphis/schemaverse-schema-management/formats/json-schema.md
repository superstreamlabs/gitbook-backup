# JSON Schema

## JSON

JSON Schema is a vocabulary that allows you to annotate and validate JSON documents.\
It provides clear human- and machine-readable documentation and offers data validation which is useful for Automated testing—ensuring the quality of client-submitted data.

### Supported Features

* Versioning
* Embedded serialization
* Producer Live evolution
* Import packages (soon)
* Import types (soon)

## Getting started

### Attach a schema

#### Step 1: Create a new schema

{% tabs %}
{% tab title="GUI" %}
Head to the "Schemaverse" page

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-10 at 15.22.17.png" alt=""><figcaption></figcaption></figure>

Create a new schema by clicking on "Create from blank"

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-01-08 at 23.21.55.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="SDK" %}
Soon.
{% endtab %}
{% endtabs %}

#### Step 2: Attach

{% tabs %}
{% tab title="GUI" %}
Head to your station, and on the top-left corner, click on "+ Attach schema"

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-10 at 16.02.31.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-10 at 16.02.38.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="SDK" %}
It can be found through the different [SDKs](broken-reference) docs.
{% endtab %}
{% endtabs %}

### Produce a message (Serialization)

{% tabs %}
{% tab title="Node.js" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

In node.js, we can simply produce an object. Behind the scenes, the object will be serialized based on the attached schema and data format - protobuf.

**Example schema:**

```protobuf
```

**Code:**

{% code lineNumbers="true" %}
```javascript
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.


{% endtab %}

{% tab title="Python" %}

{% endtab %}

{% tab title="TypeScript" %}

{% endtab %}

{% tab title=".NET" %}
Soon.
{% endtab %}
{% endtabs %}

### Consume a message (Deserialization)

{% tabs %}
{% tab title="Node.js" %}
{% code lineNumbers="true" %}
```javascript
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}

{% endtab %}

{% tab title="Python" %}

{% endtab %}

{% tab title="TypeScript" %}

{% endtab %}

{% tab title=".NET" %}

{% endtab %}
{% endtabs %}
