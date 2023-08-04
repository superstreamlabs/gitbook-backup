---
description: Accelerate Development of Real-Time Applications with Memphis.dev Cloud
cover: ../.gitbook/assets/Gitbook (1).jpeg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Getting Started

Welcome to our getting started guide with Memphis.dev cloud!&#x20;

In today's fast-paced and interconnected world, event streaming is the engine behind communication. Whether you are an individual, a small business, or an enterprise, harnessing the power of the Memphis.dev Cloud can bring unparalleled scalability, flexibility, and efficiency to your operations.&#x20;

This section aims to provide a solid foundation to embark on your streaming journey with Memphis.dev, offering clear explanations, step-by-step instructions, and valuable insights into the key concepts, services, and best practices that will empower you to make the most of the Memphis platform.&#x20;

Let's dive in and unlock the true potential of Memphis.dev together!

### Create an account

Head to the cloud [signup page](https://memphis.dev/cloud) to create an account.

<figure><img src="../.gitbook/assets/Screen Shot 2023-06-28 at 13.03.57.png" alt=""><figcaption><p>Memphis.dev cloud signup</p></figcaption></figure>

### Walkthrough

To get you up and running quickly, we created the following tutorial to guide you through the different key components. Feel free to _skip it_ if you feel comfortable enough.

<figure><img src="../.gitbook/assets/Screen Shot 2023-06-28 at 13.16.24.png" alt=""><figcaption></figcaption></figure>

#### Main overview

<figure><img src="../.gitbook/assets/Screen Shot 2023-06-28 at 13.23.27.png" alt=""><figcaption></figcaption></figure>

Upper bar -

1. **Stations:** Current amount of stations.
2. **Slow consumption stations:** Stations with a growing latency or a "lag" in one or more consumer groups.
3. **Stored messages:** Total amount of held events.
4. **Dead-letter messages:** Total amount of dead-letter messages across the system.
5. **Hostname:** Broker hostname to be used when connecting to Memphis using clients.
6. **Account ID:** Account ID to be used when connecting to Memphis using clients.

### Hello world

{% tabs %}
{% tab title="Go" %}
{% hint style="warning" %}
Full code examples can be found in this [repo](https://github.com/memphisdev/memphis.go/tree/master/examples)
{% endhint %}

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
    conn, err := memphis.Connect("BROKER_HOSTNAME", 
	"APPLICATION_TYPE_USERNAME", 
	memphis.Password("PASSWORD"), // depends on how Memphis deployed - default is connection token-based authentication
        memphis.AccountId(123456789),
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
    conn, err := memphis.Connect("BROKER_HOSTNAME", 
	"APPLICATION_TYPE_USERNAME", 
	memphis.Password("PASSWORD"), // depends on how Memphis deployed - default is connection token-based authentication
        memphis.AccountId(123456789),
        )
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
{% hint style="warning" %}
Full code examples can be found in this [repo](https://github.com/memphisdev/memphis.py/tree/master/examples)
{% endhint %}

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
import asyncio

async def main():
    try:
        memphis = Memphis()
        await memphis.connect(host="MEMPHIS_HOSTNAME", username="MEMPHIS_APPLICATION_USER", password="PASSWORD", account_id=ACCOUNT_ID)
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
import asyncio

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
        await memphis.connect(host="MEMPHIS_HOSTNAME", username="MEMPHIS_APPLICATION_USER", password="PASSWORD", account_id=ACCOUNT_ID)

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

{% tab title="Node.js" %}
{% hint style="warning" %}
Full code examples can be found in this [repo](https://github.com/memphisdev/memphis.js/tree/master/examples)
{% endhint %}

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
      accountId: ACCOUNT_ID
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
      accountId: ACCOUNT_ID
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
{% hint style="warning" %}
Full code examples can be found in this [repo](https://github.com/memphisdev/memphis.net/tree/master/examples)
{% endhint %}

**Step 1:** Create an empty dir for the new project

```bash
mkdir memphis-demo && \
cd memphis-demo
```

**Step 2:** Install memphis Node.js SDK

```bash
dotnet add package Memphis.Client
```

**Step 3:** Create a new .cs file called `Producer.cs`

{% code title="Producer.cs" lineNumbers="true" %}
```javascript
using System.Collections.Specialized;
using System.Text;
using Memphis.Client;
using Memphis.Client.Producer;

namespace Producer
{
    class ProducerApp
    {
        public static async Task Main(string[] args)
        {
            try
            {
                var options = MemphisClientFactory.GetDefaultOptions();
                options.Host = "aws-us-east-1.cloud.memphis.dev";
                options.Username = "client_type_username";
                options.Password = "client_type_password";
                var client = await MemphisClientFactory.CreateClient(options);
                options.AccountId = 123456789;

                var producer = await client.CreateProducer(new MemphisProducerOptions
                {
                    StationName = "station_name",
                    ProducerName = "producer_name",
                });

                var commonHeaders = new NameValueCollection();
                commonHeaders.Add("key-1", "value-1");

                for (int i = 0; i < 10_000000; i++)
                {
                    await Task.Delay(1_000);
                    var text = $"Message #{i}: Welcome to Memphis";
                    await producer.ProduceAsync(Encoding.UTF8.GetBytes(text), commonHeaders);
                    Console.WriteLine($"Message #{i} sent successfully");
                }

                await producer.DestroyAsync();
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine("Exception: " + ex.Message);
                Console.Error.WriteLine(ex);
            }
        }
    }
}
        
```
{% endcode %}

**Step 4:** Create a new .ts file called `Consumer.cs`

{% code title="Consumer.cs" lineNumbers="true" %}
```typescript
using System.Text;
using Memphis.Client;
using Memphis.Client.Consumer;

namespace Consumer
{
    class ConsumerApp
    {
        public static async Task Main(string[] args)
        {
            try
            {
                var options = MemphisClientFactory.GetDefaultOptions();
                options.Host = "aws-us-east-1.cloud.memphis.dev";
                options.Username = "client_type_username";
                options.Password = "client_type_password";
                var client = await MemphisClientFactory.CreateClient(options);
                options.AccountId = 123456789;

                var consumer = await client.CreateConsumer(new MemphisConsumerOptions
                {
                    StationName = "STATION_NAME",
                    ConsumerName = "CONSUMER_NAME",
                    ConsumerGroup = "CONSUMER_GROUP_NAME",
                });

                consumer.MessageReceived += (sender, args) =>
                {
                    if (args.Exception != null)
                    {
                        Console.Error.WriteLine(args.Exception);
                        return;
                    }

                    foreach (var msg in args.MessageList)
                    {
                        //print message itself
                        Console.WriteLine("Received data: " + Encoding.UTF8.GetString(msg.GetData()));


                        // print message headers
                        foreach (var headerKey in msg.GetHeaders().Keys)
                        {
                            Console.WriteLine(
                                $"Header Key: {headerKey}, value: {msg.GetHeaders()[headerKey.ToString()]}");
                        }

                        Console.WriteLine("---------");
                        msg.Ack();
                    }
                    Console.WriteLine("destroyed");
                };

                consumer.ConsumeAsync();

                // Wait 10 seconds, consumer starts to consume, if you need block main thread use await keyword.
                await Task.Delay(10_000);
                await consumer.DestroyAsync();
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine("Exception: " + ex.Message);
                Console.Error.WriteLine(ex);
            }
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title=".NET" %}
{% hint style="warning" %}
Full code examples can be found in this [repo](https://github.com/memphisdev/memphis.go/tree/master/examples)
{% endhint %}

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
    conn, err := memphis.Connect("BROKER_HOSTNAME", 
	"APPLICATION_TYPE_USERNAME", 
	memphis.Password("PASSWORD"), // depends on how Memphis deployed - default is connection token-based authentication
        memphis.AccountId(123456789),
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
    conn, err := memphis.Connect("BROKER_HOSTNAME", 
	"APPLICATION_TYPE_USERNAME", 
	memphis.Password("PASSWORD"), // depends on how Memphis deployed - default is connection token-based authentication
        memphis.AccountId(123456789),
        )
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
{% endtabs %}

