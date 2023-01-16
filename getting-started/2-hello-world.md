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

**Step 3:** Install memphis node.js SDK

```bash
npm install memphis-dev
```

**Step 4:** Create a new .js file called `producer.js`

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

**Step 5:** Run `producer.js`

```bash
node producer.js
```

**Step 6:** Create a new .js file called `consumer.js`

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

**Step 7:** Run `consumer.js`

```bash
node consumer.js
```
{% endtab %}

{% tab title="TypeScript" %}
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

**Step 3:** Install memphis node.js SDK

```bash
npm install memphis-dev
```

**Step 4:** Create a new .js file called `producer.js`
{% endtab %}

{% tab title="Go" %}
**Step 1:** Create an empty dir for the Go project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** In your project's directory, install Memphis Go SDK

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
**Step 1:** Create an empty dir for the Go project

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
import asyncio
from memphis import Memphis, Headers, MemphisError, MemphisConnectError, MemphisHeaderError, MemphisSchemaError
        
async def main():
    try:
        memphis = Memphis()
        await memphis.connect(host="MEMPHIS_HOSTNAME", username="MEMPHIS_APPLICATION_USER", connection_token="MEMPHIS_CONNECTION_TOKEN")
        
        producer = await memphis.producer(station_name="STATION_NAME", producer_name="PRODUCER_NAME")
        headers = Headers()
        headers.add("key", "value") 
        for i in range(5):
            await producer.produce(bytearray('Message #'+str(i)+': Hello world', 'utf-8'), headers=headers)
        
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
import asyncio
from memphis import Memphis, MemphisError, MemphisConnectError, MemphisHeaderError
        
async def main():
    async def msg_handler(msgs, error):
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
        await memphis.connect(host="MEMPHIS_HOSTNAME", username="MEMPHIS_APPLICATION_USER", connection_token="MEMPHIS_CONNECTION_TOKEN")
        
        consumer = await memphis.consumer(station_name="STATION_NAME", consumer_name="CONSUMER_NAME", consumer_group="CONSUMER_GROUP_NAME")
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
{% endtabs %}







