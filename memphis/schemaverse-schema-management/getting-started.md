---
cover: ../../.gitbook/assets/schema - getting started.jpeg
coverY: 0
---

# Getting Started

{% hint style="info" %}
**The current release (v0.4.0) -** Schemaverse management can be done via Memphis GUI

v0.4.1 will support SDK-driven management as well
{% endhint %}

### Step 1: Create a new schema

{% tabs %}
{% tab title="GUI" %}
Head to the "Schemaverse" page

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-10 at 15.22.17.png" alt=""><figcaption></figcaption></figure>

Create a new schema by clicking on "Create from blank"

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-10 at 15.22.25 (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Code" %}
Soon
{% endtab %}
{% endtabs %}

### Step 2: Attach Schema

{% tabs %}
{% tab title="GUI" %}
Head to your station, and on the top-left corner, click on "+ Attach schema"

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-10 at 16.02.31.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-10 at 16.02.38.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Code" %}

{% endtab %}
{% endtabs %}

### Step 3: Code examples

{% tabs %}
{% tab title="Node.js" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

#### Producer (Protobuf example)

{% code lineNumbers="true" %}
```javascript
const memphis = require("memphis-dev");
var protobuf = require("protobufjs");

(async function () {
    try {
        await memphis.connect({
            host: "localhost",
            username: "root",
            connectionToken: "memphis"
        });
        const producer = await memphis.producer({
            stationName: "marketing-partners.prod",
            producerName: "prod.1"
        });
        var payload = {
            fname: "AwesomeString",
            lname: "AwesomeString",
            id: 54,
        };
        try {
            await producer.produce({
                message: payload
        });
        } catch (ex) {
            console.log(ex.message)
        }
    } catch (ex) {
        console.log(ex);
        memphis.close();
    }
})();
```
{% endcode %}

#### Consumer (Requires .proto file to decode messages)

{% code lineNumbers="true" %}
```javascript
const memphis = require("memphis-dev");
var protobuf = require("protobufjs");

(async function () {
    try {
        await memphis.connect({
            host: "localhost",
            username: "root",
            connectionToken: "memphis"
        });

        const consumer = await memphis.consumer({
            stationName: "marketing",
            consumerName: "cons1",
            consumerGroup: "cg_cons1",
            maxMsgDeliveries: 3,
            maxAckTimeMs: 2000,
            genUniqueSuffix: true
        });

        const root = await protobuf.load("schema.proto");
        var TestMessage = root.lookupType("Test");

        consumer.on("message", message => {
            const x = message.getData()
            var msg = TestMessage.decode(x);
            console.log(msg)
            message.ack();
        });
        consumer.on("error", error => {
            console.log(error);
        });
    } catch (ex) {
        console.log(ex);
        memphis.close();
    }
})();
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

#### Producer (Protobuf example)

```
```

#### Consumer (Requires .proto file to decode messages)

{% code lineNumbers="true" %}
```javascript
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}

{% endtab %}

{% tab title="Typescript" %}

{% endtab %}
{% endtabs %}
