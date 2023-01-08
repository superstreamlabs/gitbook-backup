# Protobuf

## Google Protobuf

Protocol Buffers (Protobuf) is a free and open-source cross-platform data format used to serialize structured data, Initially released on July 7, 2008. It is useful in developing programs to communicate with each other over a network or for storing data. The method involves an interface description language that describes the structure of some data and a program that generates source code from that description for generating or parsing a stream of bytes that represents the structured data.

### Supported versions

* proto2
* proto3

### Supported Features

* Retrieve compiled protobuf schemas (Produce messages without .proto files)
* Versioning
* Embedded serialization
* Live evolution
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
syntax = "proto3";
message Test {
            string field1 = 1;
            string  field2 = 2;
            int32  field3 = 3;
}
```

**Code:**

{% code lineNumbers="true" %}
```javascript
const memphis = require("memphis-dev");

(async function () {
    try {
        await memphis.connect({
            host: "MEMPHIS_BROKER_URL",
            username: "APPLICATION_USER",
            connectionToken: "CONNECTION_TOKEN"
        });
        const producer = await memphis.producer({
            stationName: "STATION_NAME",
            producerName: "PRODUCER_NAME"
        });
        var payload = {
            fname: "AwesomeString",
            lname: "AwesomeString",
            id: 54
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
