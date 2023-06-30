# JSON Schema

[JSON Schema](https://json-schema.org/) is a vocabulary that allows you to annotate and validate JSON documents.\
It provides clear human- and machine-readable documentation and offers data validation which is useful for Automated testing—ensuring the quality of client-submitted data.

### Supported Features

* Versioning
* Embedded serialization
* Producer Live evolution
* Import packages (soon)
* Import types (soon)

### Produce/Consume a message

{% tabs %}
{% tab title="Node.js" %}
Memphis abstracts the need for external serialization functions and embeds them within the SDK.

In node.js, we can simply produce an object. Behind the scenes, the object will be serialized based on the attached schema and data format - protobuf.

**Example schema:**

```json
{
	"$schema": "http://json-schema.org/draft-04/schema#",
	"title": "contact_details",
	"type": "object",
	"properties": {
		"fname": {
			"type": "string"
		},
		"lname": {
			"type": "string"
		}
	}
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
            password: "PASSWORD",
            accountId: ACCOUNT_ID //*optional* In case you are using Memphis.dev cloud
        });
        const producer = await memphis.producer({
            stationName: "STATION_NAME",
            producerName: "PRODUCER_NAME"
        });
        var payload = {
            fname: "Daniel",
            lname: "Craig",
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

```json
{
	"$schema": "http://json-schema.org/draft-04/schema#",
	"title": "contact_details",
	"type": "object",
	"properties": {
		"fname": {
			"type": "string"
		},
		"lname": {
			"type": "string"
		}
	}
}
```

**Code:**

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
        memphis.AccountId(123456789), //*optional* In case you are using Memphis.dev cloud
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
	msg := make(map[string]interface{})
	msg["fname"] = "Daniel"
	msg["lname"] = "Craig"

    err = p.Produce(msg, memphis.MsgHeaders(hdrs))

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

```json
{
	"$schema": "http://json-schema.org/draft-04/schema#",
	"title": "contact_details",
	"type": "object",
	"properties": {
		"fname": {
			"type": "string"
		},
		"lname": {
			"type": "string"
		}
	}
}
```

**Code:**

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

    msg = '{ "fname":"John", "lname":"Mayer"}'
    msg = json.loads(msg)

    try:
        await producer.produce(msg, headers=headers)

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

```json
{
	"$schema": "http://json-schema.org/draft-04/schema#",
	"title": "contact_details",
	"type": "object",
	"properties": {
		"fname": {
			"type": "string"
		},
		"lname": {
			"type": "string"
		}
	}
}
```

**Code:**

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
            accountId: ACCOUNT_ID //*optional* In case you are using Memphis.dev cloud
        });

        const producer = await memphisConnection.producer({
            stationName: 'STATION_NAME',
            producerName: 'PRODUCER_NAME'
        });

        const headers = memphis.headers()
        headers.add('key', 'value');
        const msg = {
            fname: "Bob",
            lname: "Marley",
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

{% tab title="REST" %}
With REST, you can simply produce an object. Behind the scenes, the object will be serialized based on the attached schema and data format - protobuf.

**Example schema:**

```json
{
	"$schema": "http://json-schema.org/draft-04/schema#",
	"title": "contact_details",
	"type": "object",
	"properties": {
		"fname": {
			"type": "string"
		},
		"lname": {
			"type": "string"
		}
	}
}
```

**Producing a message:**

{% code lineNumbers="true" %}
```javascript
var axios = require('axios');
var data = JSON.stringify({
  "field1": "foo",
  "field2": "bar",
  "field3": 123,
});

var config = {
  method: 'post',
  url: 'https://BROKER_RESTGW_URL/stations/hps/produce/single',
  headers: { 
    'Authorization': 'Bearer <jwt>', 
    'Content-Type': 'application/json'
  },
  data: data
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data));
})
.catch(function (error) {
  console.log(error);
});
```
{% endcode %}
{% endtab %}

{% tab title=".NET" %}
Soon.
{% endtab %}
{% endtabs %}
