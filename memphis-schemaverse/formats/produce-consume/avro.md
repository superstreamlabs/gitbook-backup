# Avro

## Introduction

[Avro](https://avro.apache.org/) is a row-oriented remote procedure call and data serialization framework developed within Apache's Hadoop project. It uses JSON for defining data types and protocols and serializes data in a compact binary format. Its primary use is in Apache Hadoop, where it can provide both a serialization format for persistent data, and a wire format for communication between Hadoop nodes, and from client programs to the Hadoop services. Avro uses a schema to structure the data that is being encoded. It has two schema languages; one for human editing (Avro IDL) and another more machine-readable based on JSON.

## How to Produce/Consume a message

{% tabs %}
{% tab title="Node.js" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

In node.js, we can simply produce an object. Behind the scenes, the object will be serialized based on the attached schema and data format.

**Example schema:**

{% code lineNumbers="true" %}
```json
{
    "type": "record",
    "namespace": "com.example",
    "name": "contact_details",
    "fields": [
        { "name": "username", "type": "string" },
        { "name": "age", "type": "int" }
    ]
}
```
{% endcode %}

**Code:**

{% code lineNumbers="true" %}
```javascript
const memphis = require("memphis-dev");

(async function () {
    try {
        await memphis.connect({
            host: "MEMPHIS_BROKER_URL",
            username: "APPLICATION_USER",
            password: "PASSWORD",
            // accountId: ACCOUNT_ID //*optional* In case you are using Memphis.dev cloud
        });
        const producer = await memphis.producer({
            stationName: "STATION_NAME",
            producerName: "PRODUCER_NAME"
        });
        var payload = {
            username: "Daniel Craig", 
            age: 36
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

**Example schema:**

{% code lineNumbers="true" %}
```json
{
    "type": "record",
    "namespace": "com.example",
    "name": "contact_details",
    "fields": [
        { "name": "username", "type": "string" },
        { "name": "age", "type": "double" }
    ]
}
```
{% endcode %}

**Code:**

{% code lineNumbers="true" %}
```go
package main

import (
    "fmt"
    "os"
    "github.com/memphisdev/memphis.go"
)

func main() {
    conn, err := memphis.Connect(
        "MEMPHIS_BROKER_URL", 
        "APPLICATION_TYPE_USERNAME", 
        memphis.Password("PASSWORD"),
        // memphis.AccountId(123456789), //*optional* In case you are using Memphis.dev cloud
    )
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
    type msgStruct struct {
        Username string `avro:"username"`
        Age      int    `avro:"age"`
    }
    msg := msgStruct{
        Username: "Daniel Craig",
        Age:      36,
    }

    err = p.Produce(msg, memphis.MsgHeaders(hdrs))

    if err != nil {
        fmt.Printf("Produce failed: %v\n", err)
        os.Exit(1)
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

**Example schema:**

{% code lineNumbers="true" %}
```json
{
    "type": "record",
    "namespace": "com.example",
    "name": "contact_details",
    "fields": [
        { "name": "username", "type": "string" },
        { "name": "age", "type": "int" }
    ]
}
```
{% endcode %}

**Code:**

{% code lineNumbers="true" %}
```python
import asyncio
import json
from memphis import Memphis, Headers, MemphisError, MemphisConnectError, MemphisSchemaError

async def main():
    memphis = Memphis()
    await memphis.connect(host="MEMPHIS_HOST", username="MEMPHIS_USERNAME", password="PASSWORD", account_id=ACCOUNT_ID)
    producer = await memphis.producer(
        station_name="STATION_NAME", producer_name="PRODUCER_NAME")

    headers = Headers()
    headers.add("key", "value")

    msg = {'username': 'Daniel Craig', 'age': 36}

    try:
        await producer.produce(message=msg, headers=headers)

    except Exception as e:
        print(e)
    finally:
        await asyncio.sleep(3)

    await memphis.close()

if __name__ == '__main__':
    asyncio.run(main())
```
{% endcode %}
{% endtab %}

{% tab title="TypeScript" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

In node.js, we can simply produce an object. Behind the scenes, the object will be serialized based on the attached schema and data format.

**Example schema:**

{% code lineNumbers="true" %}
```json
{
    "type": "record",
    "namespace": "com.example",
    "name": "contact_details",
    "fields": [
        { "name": "username", "type": "string" },
        { "name": "age", "type": "int" }
    ]
}
```
{% endcode %}

**Code:**

{% code lineNumbers="true" %}
```typescript
import memphis from 'memphis-dev';
import type { Memphis } from 'memphis-dev/types';

(async function () {
    let memphisConnection: Memphis;

    try {
        memphisConnection = await memphis.connect({
            host: 'MEMPHIS_BROKER_URL',
            username: 'APPLICATION_TYPE_USERNAME',
            password: 'PASSWORD',
            // accountId: ACCOUNT_ID //*optional* In case you are using Memphis.dev cloud
        });

        const producer = await memphisConnection.producer({
            stationName: 'STATION_NAME',
            producerName: 'PRODUCER_NAME'
        });

        const headers = memphis.headers()
        headers.add('key', 'value');
        var msg = {
            username: "Daniel Craig", 
            age: 36
        };
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
{% endcode %}
{% endtab %}

{% endtabs %}
