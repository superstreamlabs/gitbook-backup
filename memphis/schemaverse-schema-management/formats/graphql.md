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
It can be found through the different [SDKs](broken-reference) docs.
{% endtab %}
{% endtabs %}

### Produce a message (Serialization)

{% tabs %}
{% tab title="Node.js" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

**Example schema:**

{% code lineNumbers="true" %}
```graphql
type Query {
            greeting:String
            students:[Student]
         }
         
         type Student {
            id:ID!
            firstName:String
            lastName:String
         }
```
{% endcode %}

**Code (Uint8Arrays):**

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
        const graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'
        try {
            await producer.produce({
                message: Buffer.from(graphqlMsg)
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

**Code (string):**

<pre class="language-javascript"><code class="lang-javascript"><strong>const memphis = require("memphis-dev");
</strong>
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
        const graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'
        try {
            await producer.produce({
                message: graphqlMsg
        });
        } catch (ex) {
            console.log(ex.message)
        }
    } catch (ex) {
        console.log(ex);
        memphis.close();
    }
})();
</code></pre>

**Code (DocumentNode):**

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
        const graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'
        const doc = parse(graphqlMsg)
        try {
            await producer.produce({
                message: doc
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
Soon.
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
