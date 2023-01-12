# GraphQL

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

**Code (string):**

```go
package main

import (
    "fmt"
    "os"
    "github.com/memphisdev/memphis.go"
)

func main() {
    conn, err := memphis.Connect("MEMPHIS_BROKER_URL", "APPLICATION_TYPE_USERNAME", "CONNECTION_TOKEN")
    if err != nil {
        os.Exit(1)
    }
    defer conn.Close()
    p, err := conn.CreateProducer("STATION_NAME", "PRODUCER_NAME")

    hdrs := memphis.Headers{}
    hdrs.New()
    err = hdrs.Add("key", "value")

    if err != nil {
        fmt.Printf("Header failed: %v\n", err)
        os.Exit(1)
    }
    graphQlExample := `query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}`

    err = p.Produce(graphQlExample, memphis.MsgHeaders(hdrs))

    if err != nil {
        fmt.Printf("Produce failed: %v\n", err)
        os.Exit(1)
    }
}

```

**Code (\[]byte):**

```go
package main

import (
    "fmt"
    "os"
    "github.com/memphisdev/memphis.go"
)

func main() {
    conn, err := memphis.Connect("MEMPHIS_BROKER_URL", "APPLICATION_TYPE_USERNAME", "CONNECTION_TOKEN")
    if err != nil {
        os.Exit(1)
    }
    defer conn.Close()
    p, err := conn.CreateProducer("STATION_NAME", "PRODUCER_NAME")

    hdrs := memphis.Headers{}
    hdrs.New()
    err = hdrs.Add("key", "value")

    if err != nil {
        fmt.Printf("Header failed: %v\n", err)
        os.Exit(1)
    }
    graphQlExample := `query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}`

    err = p.Produce([]byte(graphQlExample), memphis.MsgHeaders(hdrs))

    if err != nil {
        fmt.Printf("Produce failed: %v\n", err)
        os.Exit(1)
    }
}

```
{% endtab %}

{% tab title="Python" %}
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

**Code (bytearray):**

```python
import asyncio
import json
from memphis import Memphis, Headers, MemphisError, MemphisConnectError, MemphisSchemaError

async def main():
    memphis = Memphis()
    await memphis.connect(host="broker.sandbox.memphis.dev", username="json.test", connection_token="XrHmszw6rgm8IyOPNNTy")
    producer = await memphis.producer(
        station_name="json-test", producer_name="PRODUCER_NAME")

    headers = Headers()
    headers.add("key", "value")

    graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'

    try:
        await producer.produce(bytearray(graphqlExample, 'utf-8'), headers=headers)

    except Exception as e:
        print(e)
    finally:
        await asyncio.sleep(3)

    await memphis.close()

if __name__ == '__main__':
    asyncio.run(main())
```

**Code (string):**

```python
import asyncio
import json
from memphis import Memphis, Headers, MemphisError, MemphisConnectError, MemphisSchemaError

async def main():
    memphis = Memphis()
    await memphis.connect(host="broker.sandbox.memphis.dev", username="json.test", connection_token="XrHmszw6rgm8IyOPNNTy")
    producer = await memphis.producer(
        station_name="json-test", producer_name="PRODUCER_NAME")

    headers = Headers()
    headers.add("key", "value")

    graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'

    try:
        await producer.produce(graphqlMsg, headers=headers)

    except Exception as e:
        print(e)
    finally:
        await asyncio.sleep(3)

    await memphis.close()

if __name__ == '__main__':
    asyncio.run(main())
```

**Code (graphql.language.ast.DocumentNode):**

```python
import asyncio
import json
from memphis import Memphis, Headers, MemphisError, MemphisConnectError, MemphisSchemaError

async def main():
    memphis = Memphis()
    await memphis.connect(host="broker.sandbox.memphis.dev", username="json.test", connection_token="XrHmszw6rgm8IyOPNNTy")
    producer = await memphis.producer(
        station_name="json-test", producer_name="PRODUCER_NAME")

    headers = Headers()
    headers.add("key", "value")
    
    graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'
    document_node = parse(graphqlMsg)

    try:
        await producer.produce(document_node, headers=headers)

    except Exception as e:
        print(e)
    finally:
        await asyncio.sleep(3)

    await memphis.close()

if __name__ == '__main__':
    asyncio.run(main())
```
{% endtab %}

{% tab title="TypeScript" %}
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
import memphis from 'memphis-dev';
import type { Memphis } from 'memphis-dev/types';

(async function () {
    let memphisConnection: Memphis;

    try {
        memphisConnection = await memphis.connect({
            host: 'MEMPHIS_BROKER_URL',
            username: 'APPLICATION_TYPE_USERNAME',
            connectionToken: 'CONNECTION_TOKEN'
        });

        const producer = await memphisConnection.producer({
            stationName: 'STATION_NAME',
            producerName: 'PRODUCER_NAME'
        });

        const headers = memphis.headers()
        headers.add('key', 'value');
        const graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'
        
        await producer.produce({
            message: Buffer.from(graphqlMsg),
            headers: headers
        });

        memphisConnection.close();
    } catch (ex) {
        console.log(ex);
    }
})();
```

**Code (string):**

```javascript
import memphis from 'memphis-dev';
import type { Memphis } from 'memphis-dev/types';

(async function () {
    let memphisConnection: Memphis;

    try {
        memphisConnection = await memphis.connect({
            host: 'MEMPHIS_BROKER_URL',
            username: 'APPLICATION_TYPE_USERNAME',
            connectionToken: 'CONNECTION_TOKEN'
        });

        const producer = await memphisConnection.producer({
            stationName: 'STATION_NAME',
            producerName: 'PRODUCER_NAME'
        });

        const headers = memphis.headers()
        headers.add('key', 'value');
        const graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'
        await producer.produce({
            message: graphqlMsg,
            headers: headers
        });

        memphisConnection.close();
    } catch (ex) {
        console.log(ex);
    }
})();
```

**Code (DocumentNode):**

```javascript
import memphis from 'memphis-dev';
import type { Memphis } from 'memphis-dev/types';

(async function () {
    let memphisConnection: Memphis;

    try {
        memphisConnection = await memphis.connect({
            host: 'MEMPHIS_BROKER_URL',
            username: 'APPLICATION_TYPE_USERNAME',
            connectionToken: 'CONNECTION_TOKEN'
        });

        const producer = await memphisConnection.producer({
            stationName: 'STATION_NAME',
            producerName: 'PRODUCER_NAME'
        });

        const headers = memphis.headers()
        headers.add('key', 'value');
        const graphqlMsg = 'query myQuery {greeting} mutation msg { updateUserEmail( email:"http://github.com" id:1){id name}}'
        const doc = parse(graphqlMsg)
        await producer.produce({
            message: doc,
            headers: headers
        });

        memphisConnection.close();
    } catch (ex) {
        console.log(ex);
    }
})();
```
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
{% endtabs %}
