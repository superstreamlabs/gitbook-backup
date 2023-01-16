---
cover: ../.gitbook/assets/Your_streaming_journey_starts_here..jpg
coverY: 0
---

# Step 2 - Hello World

Creating your 1st station, producer, and consumer!

Please follow the steps below based on your preferred language.

{% tabs %}
{% tab title="Node.js" %}
Please make sure you have node.js [installed](https://nodejs.org/en/download/)

**Step 1:** Create an empty dir for the node.js project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** Create a new node project (If needed)

```bash
npm init -y
```

**Step 2:** Install memphis node.js SDK

```bash
npm install memphis-dev
```

**Step 3:** Create a new .js file called `producer.js`

{% code title="producer.js" lineNumbers="true" %}
```javascript
const memphis = require("memphis-dev");

(async function () {
    let memphisConnection

    try {
        memphisConnection = await memphis.connect({
            host: '<Memphis_hostname>',
            username: '<application_type_user>',
            connectionToken: '<connection_token>'
        });

        const producer = await memphisConnection.producer({
            stationName: '<station_name>',
            producerName: '<producer_name>'
        });

        const headers = memphis.headers()
        headers.add('key', 'value')
        await producer.produce({
            message: Buffer.from("Message: Hello world"),
            headers: headers
        });

        memphisConnection.close();
    } catch (ex) {
        console.log(ex);
        if (memphisConnection) memphisConnection.close();
    }
})();
        
```
{% endcode %}

**Step 4:** Run `producer.js`

```bash
node producer.js
```

**Step 5:** Create a new .js file called `consumer.js`

{% code title="consumer.js" lineNumbers="true" %}
```javascript
const memphis = require('memphis-dev');

(async function () {
    let memphisConnection;

    try {
        memphisConnection = await memphis.connect({
            host: '<Memphis_hostname>',
            username: '<application_type_user>',
            connectionToken: '<connection_token>'
        });

        const consumer = await memphisConnection.consumer({
            stationName: '<station_name',
            consumerName: '<consumer_name>',
            consumerGroup: '<consumer_group_name>'
        });

        consumer.on('message', (message) => {
            console.log(message.getData().toString());
            message.ack();
            const headers = message.getHeaders()
        });

        consumer.on('error', (error) => {});
    } catch (ex) {
        console.log(ex);
        if (memphisConnection) memphisConnection.close();
    }
})();
```
{% endcode %}

**Step 6:** Run `consumer.js`

```bash
node consumer.js
```
{% endtab %}

{% tab title="Go" %}
**Step 1:** Create an empty dir for the Go project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** In your project's directory, install Memphis:

```
go get github.com/memphisdev/memphis.go
```

**Step 3:** Create a new Go file called `producer.go`

{% code title="producer.go" lineNumbers="true" %}
```go
package main

import (
    "fmt"
    "os"

    "github.com/memphisdev/memphis.go"
)

func main() {
    conn, err := memphis.Connect("MEMPHIS_HOSTNAME", "MEMPHIS_APPLICATION_USER", "MEMPHIS_CONNECTION_TOKEN")
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
    "os"
    "time"

    "github.com/memphisdev/memphis.go"
)

func main() {
    conn, err := memphis.Connect("MEMPHIS_HOSTNAME", "MEMPHIS_APPLICATION_USER", "MEMPHIS_CONNECTION_TOKEN")
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

    handler := func(msgs []*memphis.Msg, err error) {
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
{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}



#### Step 1: Install Memphis py SDK

Within your code directory, run

```
pip3 install memphis-py
```

#### Step 2: Import Memphis SDK to your code

```
from memphis import Memphis
from memphis import retention_types, storage_types
```

#### Step 3: Run a code example

Don't forget to replace the placeholders

```
async def main():
  try:
    memphis = Memphis()
    await memphis.connect(
      host="<memphis-host>",
      username="<application-type username>",
      connection_token="<broker-token>",
      port="<port>", # defaults to 6666
      reconnect=True, # defaults to False
      max_reconnect=10, # defaults to 10
      reconnect_interval_ms=1500, # defaults to 1500
      timeout_ms=1500 # defaults to 1500
      )
  except Exception as e:
    print(e)
  finally:
    await memphis.close()

if __name__ == '__main__':
  asyncio.run(main())
```
{% endtab %}

{% tab title="TypeScript" %}

{% endtab %}
{% endtabs %}







