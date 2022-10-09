# 2 - Hello World

No installation is needed!

For this tutorial, we will use [sandbox.memphis.dev](https://sandbox.memphis.dev)

You can also use this link for a live [demo app](https://demo.memphis.dev).



{% tabs %}
{% tab title="Node.JS / Typescript" %}
Please make sure you have node.js [installed](https://nodejs.org/en/download/)



**Step 0:** Create an empty dir for node.js project

```
mkdir memphis-demo && \
cd memphis-demo
```

**Step 1:** Create a new node project (If needed)

```
npm init -y
```

**Step 2:** Install memphis node.js SDK

```
npm install memphis-dev
```

**Step 3:** Create a new text file called `index.js`

```
const memphis = require("memphis-dev");

(async function () {
    try {
        await memphis.connect({
            host: "broker.sandbox.memphis.dev",
            username: "root",
            connectionToken: "XrHmszw6rgm8IyOPNNTy"
        });
        
        // consumer
        
        const consumer = await memphis.consumer({
            stationName: "hello_world",
            consumerName: "consumer_app1",
            consumerGroup: "consumer_group1"
        });
        consumer.on("message", message => {
            console.log(message.getData().toString());
            message.ack();
        });
        consumer.on("error", error => {
            console.log(error);
        });

        // producer

        const producer = await memphis.producer({
            stationName: "hello_world",
            producerName: "producer_app"
        });
        for (let index = 0; index < 100; index++) {
            await producer.produce({
                message: Buffer.from('Hello world')
            });
            console.log("Message sent");
        }
        console.log("All messages sent");
    } catch (ex) {
        console.log(ex);
        memphis.close();
    }
})();
```

```
node index.js
```
{% endtab %}

{% tab title="Go" %}
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
{% endtabs %}



