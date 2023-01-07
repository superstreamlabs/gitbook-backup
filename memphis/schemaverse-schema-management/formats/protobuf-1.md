# GraphQL

## GraphQL

GraphQL is an open-source data query and manipulation language for APIs, and a runtime for fulfilling queries with existing data. GraphQL was developed internally by Facebook in 2012 before being publicly released in 2015. On 7 November 2018, the GraphQL project was moved from Facebook to the newly established GraphQL Foundation, hosted by the non-profit Linux Foundation.

## Getting started

### Attach a schema

#### Step 1: Create a new schema

{% tabs %}
{% tab title="GUI" %}
Head to the "Schemaverse" page

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-10 at 15.22.17.png" alt=""><figcaption></figcaption></figure>

Create a new schema by clicking on "Create from blank"

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-10 at 15.22.25 (1).png" alt=""><figcaption></figcaption></figure>
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
Soon.
{% endtab %}
{% endtabs %}

### Produce a message (Serialization)

{% tabs %}
{% tab title="Node.js" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

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

### Consume a message (Deserialization)

{% tabs %}
{% tab title="Node.js" %}


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

{% endtab %}

{% tab title="Python" %}

{% endtab %}

{% tab title="TypeScript" %}

{% endtab %}

{% tab title=".NET" %}

{% endtab %}
{% endtabs %}
