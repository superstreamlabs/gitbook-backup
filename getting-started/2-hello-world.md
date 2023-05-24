---
cover: ../.gitbook/assets/LinkedIn personal (3).png
coverY: 0
---

# Step 2 - Hello World

Create your first station, producer, and consumer in your preferred language as shown in the tabs below:

{% tabs %}
{% tab title="Node.js" %}
Please make sure you have Node.js [installed](https://nodejs.org/en/download/).

**Step 1:** Create an empty dir for the Node.js project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** Create a new Node project (If needed)

```bash
npm init -y
```

**Step 3:** Install memphis Node.js SDK

```bash
npm install memphis-dev
```

**Step 4:** Create a new .js file called `producer.js`

{% code title="producer.js" lineNumbers="true" %}

```javascript
const { memphis } = require("memphis-dev");

(async function () {
  let memphisConnection;

  try {
    memphisConnection = await memphis.connect({
      host: "MEMPHIS_BROKER_HOSTNAME",
      username: "APPLICATION_TYPE_USERNAME",
      password: "PASSWORD",
    });

    const producer = await memphisConnection.producer({
      stationName: "STATION_NAME",
      producerName: "PRODUCER_NAME",
    });

    const headers = memphis.headers();
    headers.add("KEY", "VALUE");
    await producer.produce({
      message: Buffer.from("Message: Hello world"), // you can also send JS object - {}
      headers: headers,
    });

    memphisConnection.close();
  } catch (ex) {
    console.log(ex);
    if (memphisConnection) memphisConnection.close();
  }
})();
```

{% endcode %}

**Step 5:** Run `producer.js`

```bash
node producer.js
```

**Step 6:** Create a new .js file called `consumer.js`

{% code title="consumer.js" lineNumbers="true" %}

```javascript
const { memphis } = require("memphis-dev");

(async function () {
  let memphisConnection;

  try {
    memphisConnection = await memphis.connect({
      host: "MEMPHIS_BROKER_HOSTNAME",
      username: "APPLICATION_TYPE_USERNAME",
      password: "PASSWORD",
    });

    const consumer = await memphisConnection.consumer({
      stationName: "STATION_NAME",
      consumerName: "CONSUMER_NAME",
      consumerGroup: "CONSUMER_GROUP_NAME",
    });

    consumer.setContext({ key: "value" });
    consumer.on("message", (message, context) => {
      console.log(message.getData().toString());
      message.ack();
      const headers = message.getHeaders();
    });

    consumer.on("error", (error) => {});
  } catch (ex) {
    console.log(ex);
    if (memphisConnection) memphisConnection.close();
  }
})();
```

{% endcode %}

**Step 7:** Run `consumer.js`

```bash
node consumer.js
```

{% endtab %}

{% tab title="TypeScript" %}
Please make sure you have Node.js [installed](https://nodejs.org/en/download/).

**Step 1:** Create an empty dir for the TypeScript project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** Create a new Node project (If needed)

```bash
npm init -y
```

**Step 3:** Install memphis Node.js SDK

```bash
npm install memphis-dev
```

**Step 4:** Create a new .ts file called `producer.ts`

{% code title="producer.ts" lineNumbers="true" %}

```typescript
import { memphis, Memphis } from "memphis-dev";

(async function () {
  let memphisConnection: Memphis;

  try {
    memphisConnection = await memphis.connect({
      host: "MEMPHIS_BROKER_HOSTNAME",
      username: "APPLICATION_TYPE_USERNAME",
      password: "PASSWORD",
    });

    const producer = await memphisConnection.producer({
      stationName: "STATION_NAME",
      producerName: "PRODUCER_NAME",
    });

    const headers = memphis.headers();
    headers.add("key", "value");
    await producer.produce({
      message: Buffer.from("Message: Hello world"), // you can also send JS object - {}
      headers: headers,
    });

    memphisConnection.close();
  } catch (ex) {
    console.log(ex);
    if (memphisConnection) memphisConnection.close();
  }
})();
```

{% endcode %}

**Step 5:** Run `producer.ts`

```bash
node producer.ts
```

**Step 6:** Create a new .ts file called `consumer.ts`

{% code title="consumer.ts" lineNumbers="true" %}

```typescript
import { memphis, Memphis } from "memphis-dev";

(async function () {
  let memphisConnection: Memphis;

  try {
    memphisConnection = await memphis.connect({
      host: "MEMPHIS_BROKER_HOSTNAME",
      username: "APPLICATION_TYPE_USERNAME",
      password: "PASSWORD",
    });

    const consumer = await memphisConnection.consumer({
      stationName: "STATION_NAME",
      consumerName: "CONSUMER_NAME",
      consumerGroup: "CONSUMER_GROUP_NAME",
    });

    consumer.setContext({ key: "value" });
    consumer.on("message", (message: Message, context: object) => {
      console.log(message.getData().toString());
      message.ack();
      const headers = message.getHeaders();
    });

    consumer.on("error", (error) => {
      console.log(error);
    });
  } catch (ex) {
    console.log(ex);
    if (memphisConnection) memphisConnection.close();
  }
})();
```

{% endcode %}

**Step 7:** Run `consumer.ts`

```bash
node consumer.ts
```

{% endtab %}

{% tab title="NestJS" %}
Please make sure you have Node.js [installed](https://nodejs.org/en/download/).

**Step 1:** Create an empty dir for the NestJS project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** Create a new Node project (If needed)

```bash
npm init -y
```

**Step 3:** Install memphis Node.js SDK

```bash
npm install memphis-dev
```

**Step 4:** Create a new .ts file called `producer.module.ts`

{% code title="producer.module.ts" lineNumbers="true" %}

```typescript
import { Module } from "@nestjs/common";
import { Memphis, MemphisModule, MemphisService } from "memphis-dev";

@Module({
  imports: [MemphisModule.register()],
})
export class ProducerModule {
  constructor(private memphis: MemphisService) {}

  startProducer() {
    (async function () {
      let memphisConnection: Memphis;

      try {
        memphisConnection = await memphis.connect({
          host: "MEMPHIS_BROKER_HOSTNAME",
          username: "APPLICATION_TYPE_USERNAME",
          password: "PASSWORD",
        });

        const producer = await memphisConnection.producer({
          stationName: "STATION_NAME",
          producerName: "PRODUCER_NAME",
        });

        for (let index = 0; index < 100; index++) {
          await producer.produce({
            message: Buffer.from(`Message #${index}: Hello world`), // you can also send JS object - {}
          });
          console.log("Message sent");
        }

        console.log("All messages sent");
        memphisConnection.close();
      } catch (ex) {
        console.log(ex);
        if (memphisConnection) memphisConnection.close();
      }
    })();
  }
}
```

{% endcode %}

**Step 5:** Create a new .ts file called `consumer.controller.ts`

{% code title="consumer.controller.ts" lineNumbers="true" %}

```typescript
import { Module } from '@nestjs/common';
import { Memphis, MemphisModule, MemphisService, MemphisConsume, Message } from 'memphis-dev';

@Controller("auth")
export class ExampleController {
    @MemphisConsume({
        stationName: "STATION_NAME",
        consumerName: "CONSUMER_NAME",
        consumerGroup: "CONSUMER_GROUP_NAME",
    }, {}) // {} for passing the consumerContext to consumer.setContext
    async messageHandler(message: Message, context: object) {
        console.log(message.getData().toString());
        message.ack();
    }
}
```

{% endcode %}
{% endtab %}

{% tab title="Go" %}
**Step 1:** Create an empty dir for the Go project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** Init the newly created project

```
go mod init memphis-demo
```

**Step 3:** In your project's directory, install Memphis Go SDK

```
go get github.com/memphisdev/memphis.go
```

**Step 4:** Create a new Go file called `producer.go`

{% code title="producer.go" lineNumbers="true" %}

```go
package main

import (
    "fmt"
    "os"

    "github.com/memphisdev/memphis.go"
)

func main() {
    conn, err := memphis.Connect("MEMPHIS_HOSTNAME", "MEMPHIS_APPLICATION_USER", memphis.Password("PASSWORD"))
    if err != nil {
        os.Exit(1)
    }
    defer conn.Close()
    p, err := conn.CreateProducer("STATION_NAME", "PRODUCER_NAME")

    hdrs := memphis.Headers{}
    hdrs.New()
    err = hdrs.Add("key", "value")

    if err != nil {
        fmt.Errorf("Header failed: %v", err)
        os.Exit(1)
    }

    err = p.Produce([]byte("You have a message!"), memphis.MsgHeaders(hdrs))

    if err != nil {
        fmt.Errorf("Produce failed: %v", err)
        os.Exit(1)
    }
}
```

{% endcode %}

**Step 4:** Run `producer.go`

```bash
go run producer.go
```

**Step 5:** Create a new Go file called `consumer.go`

{% code title="consumer.go" lineNumbers="true" %}

```go
package main

import (
    "fmt"
    "context"
    "os"
    "time"

    "github.com/memphisdev/memphis.go"
)

func main() {
    conn, err := memphis.Connect("MEMPHIS_HOSTNAME", "MEMPHIS_APPLICATION_USER", memphis.Password("PASSWORD"))
    if err != nil {
        os.Exit(1)
    }
    defer conn.Close()

    consumer, err := conn.CreateConsumer("STATION_NAME", "CONSUMER_NAME", memphis.PullInterval(15*time.Second))

    if err != nil {
        fmt.Printf("Consumer creation failed: %v
", err)
        os.Exit(1)
    }

    handler := func(msgs []*memphis.Msg, err error, ctx context.Context) {
        if err != nil {
            fmt.Printf("Fetch failed: %v
", err)
            return
        }

        for _, msg := range msgs {
            fmt.Println(string(msg.Data()))
            msg.Ack()
            headers := msg.GetHeaders()
            fmt.Println(headers)
        }
    }

    ctx := context.Background()
    ctx = context.WithValue(ctx, "key", "value")
    consumer.SetContext(ctx)
    consumer.Consume(handler)

    // The program will close the connection after 30 seconds,
    // the message handler may be called after the connection closed
    // so the handler may receive a timeout error
    time.Sleep(30 * time.Second)
}
```

{% endcode %}

**Step 6:** Run `consumer.go`

```bash
go run consumer.go
```

{% endtab %}

{% tab title="Python" %}
**Step 1:** Create an empty dir for the Python project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** In your project's directory, install Memphis Python SDK

```bash
pip3 install --upgrade memphis-py
```

**Step 3:** Create a new Python file called `producer.py`

{% code title="producer.py" lineNumbers="true" %}

```python
from memphis import Memphis, Headers
from memphis.types import Retention, Storage

async def main():
    try:
        memphis = Memphis()
        await memphis.connect(host="MEMPHIS_HOSTNAME", username="MEMPHIS_APPLICATION_USER", password="PASSWORD")

        producer = await memphis.producer(station_name="STATION_NAME", producer_name="PRODUCER_NAME")
        headers = Headers()
        headers.add("key", "value")
        for i in range(5):
            await producer.produce(bytearray('Message #'+str(i)+': Hello world', 'utf-8'), headers=headers) # you can send the message parameter as dict as well

    except (MemphisError, MemphisConnectError, MemphisHeaderError, MemphisSchemaError) as e:
        print(e)

    finally:
        await memphis.close()

if __name__ == '__main__':
    asyncio.run(main())
```

{% endcode %}

**Step 4:** Run `producer.py`

```bash
python3 producer.py
```

**Step 5:** Create a new Python file called `consumer.py`

{% code title="consumer.py" lineNumbers="true" %}

```python
from memphis import Memphis, Headers
from memphis.types import Retention, Storage

async def main():
    async def msg_handler(msgs, error, context):
        try:
            for msg in msgs:
                print("message: ", msg.get_data())
                await msg.ack()
                headers = msg.get_headers()
                if error:
                    print(error)
        except (MemphisError, MemphisConnectError, MemphisHeaderError) as e:
            print(e)
            return

    try:
        memphis = Memphis()
        await memphis.connect(host="MEMPHIS_HOSTNAME", username="MEMPHIS_APPLICATION_USER", password="PASSWORD")

        consumer = await memphis.consumer(station_name="STATION_NAME", consumer_name="CONSUMER_NAME", consumer_group="CONSUMER_GROUP_NAME")
        consumer.set_context({"key": "value"})
        consumer.consume(msg_handler)
        # Keep your main thread alive so the consumer will keep receiving data
        await asyncio.Event().wait()

    except (MemphisError, MemphisConnectError) as e:
        print(e)

    finally:
        await memphis.close()

if __name__ == '__main__':
    asyncio.run(main())
```

{% endcode %}

**Step 6:** Run `consumer.py`

```bash
python3 consumer.py
```

{% endtab %}

{% tab title="REST" %}
Producing messages to Memphis via REST API can be implemented using any REST-supported language like Go, Python, Java, Node.js, .NET, etc...

For the following tutorial, we will use Node.js .

**Step 1:** Create an empty dir for the REST API project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** Create a new Node project (If needed)

```bash
npm init -y
```

**Step 3:** Generate a new JWT token `generate.js`

{% code title="generate.js" lineNumbers="true" %}

```javascript
var axios = require("axios");
var data = JSON.stringify({
  username: "APPLICATION_TYPE_USERNAME",
  password: "PASSWORD",
  token_expiry_in_minutes: 123,
  refresh_token_expiry_in_minutes: 10000092,
});

var config = {
  method: "post",
  url: "BROKER_RESTGW_URL",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
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

**Step 4:** Run `generate.js` and copy the returned JWT

```bash
node generate.js
```

**Step 5:** Create a new file called `producer.js`

```javascript
var axios = require("axios");
var data = JSON.stringify({
  message: "New Message",
});

var config = {
  method: "post",
  url: "https://BROKER_RESTGW_URL/stations/hps/produce/single",
  headers: {
    Authorization: "Bearer <jwt>",
    "Content-Type": "application/json",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

**Consume** messages via REST will soon be released.
{% endtab %}
{% endtabs %}
