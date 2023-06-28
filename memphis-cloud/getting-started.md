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

Welcome to our comprehensive guide on getting started with Memphis.dev cloud!&#x20;

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
{% hint style="info" %}
The full code example can be found here
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

{% endtab %}

{% tab title="Node.js" %}

{% endtab %}

{% tab title="TypeScript" %}

{% endtab %}

{% tab title="NestJS" %}

{% endtab %}

{% tab title="REST" %}

{% endtab %}
{% endtabs %}

