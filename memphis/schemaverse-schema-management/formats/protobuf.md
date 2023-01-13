# Protobuf

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

**Producing a message **<mark style="color:purple;">**without**</mark>** a local .proto file:**

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
Memphis abstracts the need for external serialization functions and embeds it within the SDK.

**Example schema:**

```protobuf
syntax = "proto3";
message Test {
            string field1 = 1;
            string field2 = 2;
            int32 field3 = 3;
}
```

**Producing a message **<mark style="color:purple;">**without**</mark>** a local .proto file:**

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
	msg := make(map[string]interface{})
	msg["field1"] = "value1"
	msg["field2"] = "value2"
	msg["field3"] = 32

    err = p.Produce(msg, memphis.MsgHeaders(hdrs))

    if err != nil {
        fmt.Printf("Produce failed: %v\n", err)
        os.Exit(1)
    }
}

```

**Producing a message **<mark style="color:purple;">**with**</mark>** a local .proto file:**

```go
package main

import (
    "fmt"
    "os"
    "demo/schemapb"
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
    s1 := "Hello"
    s2 := "World"
    pbInstance := schemapb.Test{
	Field1: &s1,
	Field2: &s2,
    }

    err = p.Produce(&pbInstance, memphis.MsgHeaders(hdrs))

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

```protobuf
syntax = "proto3";
message Test {
            string field1 = 1;
            string field2 = 2;
            int32 field3 = 3;
}
```

**Producing a message **<mark style="color:purple;">**with**</mark>** a local .proto file:**

```python
import asyncio
from memphis import Memphis, Headers, MemphisError, MemphisConnectError, MemphisSchemaError

import schema_pb2 as PB

async def main():
    memphis = Memphis()
    await memphis.connect(host="MEMPHIS_BROKER_URL", username="APPLICATION_TYPE_USERNAME", connection_token="CONNECTION_TOKEN")
    producer = await memphis.producer(
        station_name="STATION_NAME", producer_name="PRODUCER_NAME")

    headers = Headers()
    headers.add("key", "value")

    obj = PB.Test()
    obj.field1 = "Hello"
    obj.field2 = "Amazing"
    obj.field3 = "World"
    
    try:
        await producer.produce(obj, headers=headers)

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

```protobuf
syntax = "proto3";
message Test {
            string field1 = 1;
            string field2 = 2;
            int32 field3 = 3;
}
```

**Producing a message **<mark style="color:purple;">**without**</mark>** a local .proto file:**

```typescript
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
        const msg = {
            field1: "Hello",
            field2: "Amazing",
            field3: "World"
        }
        await producer.produce({
            message: msg,
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
{% endtabs %}
