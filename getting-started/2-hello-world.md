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

**Step 4:** Run `producer.js`

```bash
node producer.js
```

**Step 5:** Create a new .js file called `consumer.js`

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

**Step 6:** Run `consumer.js`

```bash
node consumer.js
```
{% endtab %}

{% tab title="Go" %}
**Step 1:** Create an empty dir for the node.js project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 1:** In your project's directory:

```
go get github.com/memphisdev/memphis.go
```

**Step 2:** Importing

```
import "github.com/memphisdev/memphis.go"
```

**Step 3:** Connecting

```
c, err := memphis.Connect("<memphis-host>", "<application type username>", "<broker-token>")
	if err != nil {
		os.Exit(1)
	}
	defer c.Close()
```

**Complete code example**

```
package main

    import (
        "fmt"
        "os"
        "time"
    
        "github.com/memphisdev/memphis.go"
    )
    
    func main() {
        conn, err := memphis.Connect("broker.sandbox.memphis.dev", "<application type username>", "<connection_token>")
        if err != nil {
            os.Exit(1)
        }
        defer conn.Close()
    
        // producer
        p, err := conn.CreateProducer("sh", "myProducer")
        if err != nil {
            fmt.Printf("Producer creation failed: %v
", err)
            os.Exit(1)
        }
    
        err = p.Produce([]byte("You have a message!"))
        if err != nil {
            fmt.Errorf("Produce failed: %v", err)
            os.Exit(1)
        }
    
        // consumer
        consumer, err := conn.CreateConsumer("sh", "myConsumer", memphis.PullInterval(15*time.Second))
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
            }
        }
    
        consumer.Consume(handler)
        time.Sleep(30 * time.Second)
    }
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







