---
description: Create your first station, producer, and consumer in your preferred language.
cover: ../.gitbook/assets/Docs (3).png
coverY: 0
---

# Quick start

{% hint style="info" %}
To learn by practice, you can find a sample application [**here**](https://github.com/memphisdev/onboarding-app).
{% endhint %}

## 1. Which Memphis deployment are you using?

{% tabs %}
{% tab title="Cloud" %}
**Step 1:** Sign up for Memphis Cloud [here](https://cloud.memphis.dev).

**Step 2:** [Hello world](../memphis-cloud/getting-started.md#hello-world)
{% endtab %}

{% tab title="Open-source" %}
### **Installation**

#### **For Kubernetes**

Stable -

{% code lineNumbers="true" %}
```bash
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && 
helm install memphis memphis/memphis --create-namespace --namespace memphis --wait --version=1.2.2
```
{% endcode %}

Latest -

<pre class="language-bash" data-line-numbers><code class="lang-bash">helm repo add memphis https://k8s.memphis.dev/charts/ --force-update &#x26;&#x26; 
<strong>helm install memphis memphis/memphis --create-namespace --namespace memphis --wait
</strong></code></pre>

More information can be found in the [Memphis k8s deployment](../open-source-installation/kubernetes/) documentation.

#### **Docker compose (Syntax for v2)**

Stable -&#x20;

{% code overflow="wrap" %}
```bash
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```
{% endcode %}

Latest -

{% code overflow="wrap" %}
```bash
curl -s https://memphisdev.github.io/memphis-docker/docker-compose-latest.yml -o docker-compose-latest.yml && docker compose -f docker-compose-latest.yml -p memphis up
```
{% endcode %}

More information can be found in the [Memphis Docker deployment](../open-source-installation/docker-compose/) documentation.
{% endtab %}
{% endtabs %}

## 2. Hello world

{% tabs %}
{% tab title="Node.js" %}
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

    consumer.setContext({ key: "value" }); // Optional

    let messages = await consumer.fetch(); // Fetches 10 messages

    for (let message of messages){
        const messageObject = JSON.parse(message.getData().toString());
        // Do something with the message
        console.log(messageObject["Hello"]);
        message.ack();
    }

    memphisConnection.close();

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
import { memphis, Memphis, Message } from "memphis-dev";

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

    let messages: Message[] = await consumer.fetch(); // Fetches 10 messages

    for (let message of messages){
        const messageObject: Record<string, any> = JSON.parse(message.getData().toString());
        // Do something with the message
        console.log(messageObject["Hello"]);
        message.ack();
    }

    memphisConnection.close();

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
    conn, err := memphis.Connect("MEMPHIS_HOSTNAME", "MEMPHIS_APPLICATION_USER", memphis.Password("PASSWORD"),)
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
    conn, err := memphis.Connect("MEMPHIS_HOSTNAME", "MEMPHIS_APPLICATION_USER", memphis.Password("PASSWORD"),)
    if err != nil {
        os.Exit(1)
    }
    defer conn.Close()

    consumer, err := conn.CreateConsumer("STATION_NAME", "CONSUMER_NAME", memphis.PullInterval(15*time.Second))

    if err != nil {
        fmt.Printf("Consumer creation failed: %v", err)
        os.Exit(1)
    }

    consumer.SetContext(ctx)
    
    messages, err := consumer.Fetch()

    var msg_map map[string]any
    for _, message := range messages{
      err = json.Unmarshal(message.Data(), &msg_map)
      if err != nil{
        fmt.Print(err)
        continue
      }

      // Do something with the message

      err = message.Ack()
      if err != nil{
        fmt.Print(err)
        continue
      }
    }

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
import asyncio

async def main():
    try:
        memphis = Memphis()
        await memphis.connect(host="localhost", username="root", password="memphis")
        producer = await memphis.producer(station_name="memphis-test", producer_name="producer-test")
        await producer.produce(bytearray('Hello world', 'utf-8')) # you can send the message parameter as dict as well

    except Exception as e:
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
from memphis.message import Message
import asyncio

async def main():
    try:
        memphis = Memphis()
        await memphis.connect(host="MEMPHIS_HOSTNAME", username="MEMPHIS_APPLICATION_USER", password="PASSWORD")
        consumer = await memphis.consumer(station_name="STATION_NAME", consumer_name="CONSUMER_NAME", consumer_group="CONSUMER_GROUP_NAME")
        
        messages: list[Message] = await consumer.fetch() # Type-hint the return here for LSP integration
        
        for consumed_message in messages:
            msg_data = json.loads(consumed_message.get_data())

            # Do something with the message data

            await consumed_message.ack()

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

{% tab title="C#" %}

**Step 1:** Create a new C# project through Visual Studio or using the dotnet CLI

```bash
dotnet new console -n MyC#Project
```

If you are using top level statements, you will have to create two of these projects.

**Step 2:** In your project's directory(s), install Memphis C# SDK

```bash
dotnet add package Memphis.Client -v ${MEMPHIS_CLIENT_VERSION}
```

**Step 3:** In your Producer file/project copy this quickstart inside of it

{% code title="producer.cs" lineNumbers="true" %}
```C#
using Memphis.Client;
using System.Collections.Specialized;
using System.Text;
using System.Text.Json;

try
{
    var options = MemphisClientFactory.GetDefaultOptions();
    options.Host = "aws-us-east-1.cloud.memphis.dev";
    options.AccountId = int.Parse(Environment.GetEnvironmentVariable("memphis_account_id"));
    options.Username = "test_user";
    options.Password = Environment.GetEnvironmentVariable("memphis_pass");

    var memphisClient = await MemphisClientFactory.CreateClient(options);

    var producer = await memphisClient.CreateProducer(
    new Memphis.Client.Producer.MemphisProducerOptions
    {
        StationName = "test_station",
        ProducerName = "producer"
    });

    Message message = new()
    {
        Hello = "World!"
    };

    var headers = new NameValueCollection();

    for (int i = 0; i < 3; i++)
    {
        var msgBytes = Encoding.UTF8.GetBytes(JsonSerializer.Serialize(message));
        await producer.ProduceAsync(msgBytes, headers);
    }

    memphisClient.Dispose();
}
catch (Exception ex)
{
    Console.Error.WriteLine(ex.Message);
}

public class Message
{
    public string Hello { get; set; }
}
```
{% endcode %}

**Step 4:** Run `producer.cs`

```bash
dotnet run
```

**Step 5:** Create a new C# project/file called `consumer.cs`

{% code title="consumer.cs" lineNumbers="true" %}
```c#
using Memphis.Client;
using Memphis.Client.Core;
using System.Text.Json;

try
{
    var options = MemphisClientFactory.GetDefaultOptions();
    options.Host = "aws-us-east-1.cloud.memphis.dev";
    options.AccountId = int.Parse(Environment.GetEnvironmentVariable("memphis_account_id"));
    options.Username = "test_user";
    options.Password = Environment.GetEnvironmentVariable("memphis_pass");

    var memphisClient = await MemphisClientFactory.CreateClient(options);

    var consumer = await memphisClient.CreateConsumer(
       new Memphis.Client.Consumer.MemphisConsumerOptions
       {
           StationName = "test_station",
           ConsumerName = "consumer"
       });

    var messages = consumer.Fetch(3, false);

    foreach (MemphisMessage message in messages)
    {
        var messageData = message.GetData();
        var messageOBJ = JsonSerializer.Deserialize<Message>(messageData);

        // Do something with the message here
        Console.WriteLine(JsonSerializer.Serialize(messageOBJ));

        message.Ack();
    }

    memphisClient.Dispose();

}
catch (Exception ex)
{
    Console.Error.WriteLine(ex.Message);
}

public class Message
{
    public string Hello { get; set; }
}
```
{% endcode %}

**Step 6:** Run `consumer.cs`

```bash
dotnet run
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
  url: "localhost:4444",
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
  url: "http://localhost:4444/stations/hps/produce/single",
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

More code examples [here](https://github.com/memphisdev/memphis-rest-gateway)
{% endtab %}
{% endtabs %}
