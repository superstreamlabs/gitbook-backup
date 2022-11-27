# Go

{% content-ref url="example-producer.md" %}
[example-producer.md](example-producer.md)
{% endcontent-ref %}

{% content-ref url="example-consumer.md" %}
[example-consumer.md](example-consumer.md)
{% endcontent-ref %}

## Installation

{% hint style="info" %}
First, install Memphis via [K8S](../../deployment/kubernetes/) / [Docker](../../deployment/docker-compose.md).
{% endhint %}

In your project's directory:

```
go get github.com/memphisdev/memphis.go
```

## Importing

```
import "github.com/memphisdev/memphis.go"
```

### Connecting to Memphis

```go
c, err := memphis.Connect("<memphis-host>", 
	"<application type username>", 
	"<broker-token>")
```

It is possible to pass connection configuration parameters, as function-parameters.

```go
// function params
c, err := memphis.Connect("<memphis-host>", 
	"<application type username>", 
	"<broker-token>",
	Port(<int>),        
	Reconnect(<bool>),
	MaxReconnect(<int>)
	)
```

Once connected, all features offered by Memphis are available.

### Disconnecting from Memphis

To disconnect from Memphis, call `Close()` on the Memphis connection object.

```go
c.Close();
```

### Creating a Station

Stations can be created from `Conn`

Passing optional parameters using functions&#x20;

<pre class="language-go"><code class="lang-go">s0, err = c.CreateStation("&#x3C;station-name>")

s1, err = c.CreateStation("&#x3C;station-name>", 
 RetentionTypeOpt(&#x3C;Messages/MaxMeMessageAgeSeconds/Bytes>),
<strong> RetentionVal(&#x3C;int>), 
</strong> StorageTypeOpt(&#x3C;Memory/Disk>), 
 Replicas(&#x3C;int>), 
 EnableDedup(), 
 DedupWindow(&#x3C;time.Duration>))</code></pre>

### Retention Types

Memphis currently supports the following types of retention:

```go
memphis.MaxMeMessageAgeSeconds
```

The above means that every message persists for the value set in the retention value field (in seconds).

```go
memphis.Messages
```

The above means that after the maximum number of saved messages (set in retention value) has been reached, the oldest messages will be deleted.

```
memphis.Bytes
```

The above means that after maximum number of saved bytes (set in retention value) has been reached, the oldest messages will be deleted.

### Storage Types

Memphis currently supports the following types of messages storage:

```
memphis.Disk
```

The above means that messages persist on disk.

```
memphis.Memory
```

The above means that messages persist on the main memory.

### Destroying a Station

Destroying a station will remove all its resources (including producers and consumers).

```go
err := s.Destroy();
```

### Produce and Consume Messages

The most common client operations are producing messages and consuming messages.

Messages are published to a station and consumed from it by creating a consumer and calling its Consume function with a message handler callback function. Consumers are pull-based and consume all the messages in a station unless you are using a consumers group, in which case messages are spread across all members in this group.

Memphis messages are payload agnostic. Payloads are byte slices, i.e `[]byte`.

In order to stop receiving messages, you have to call `consumer.StopConsume()`. The consumer will terminate regardless of whether there are messages in flight for the client.

### Creating a Producer

```go
// from a Conn
p0, err := c.CreateProducer(
	"<station-name>",
	"<producer-name>",
	memphis.ProducerGenUniqueSuffix()
) 

// from a Station
p1, err := s.CreateProducer("<producer-name>")
```

### Producing a Message

```go
p.Produce("<message in []byte/protoreflect.ProtoMessage in case it is a schema validated station>",
memphis.AckWaitSec(15)) // defaults to 15 seconds
```

### Add headers

```
hdrs := memphis.Headers{}
hdrs.New()
err := hdrs.Add("key", "value")
p.Produce(
	"<message in []byte>/protoreflect.ProtoMessage in case it is a schema validated station",
    memphis.AckWaitSec(15),
	memphis.MsgHeaders(hdrs) // defaults to empty
)
```

### Async produce

Meaning your application won't wait for broker acknowledgement - use only in case you are tolerant for data loss

```
p.Produce(
	"<message in []byte>/protoreflect.ProtoMessage in case it is a schema validated station",
    memphis.AckWaitSec(15),
	memphis.AsyncProduce()
)
```

### Destroying a Producer

```go
p.Destroy();
```

### Creating a Consumer

```go
// creation from a Station
consumer0, err = s.CreateConsumer("<consumer-name>",
  memphis.ConsumerGroup("<consumer-group>"), // defaults to consumer name
  memphis.PullInterval(<pull interval time.Duration), // defaults to 1 second
  memphis.BatchSize(<batch-size int), // defaults to 10
  memphis.BatchMaxWaitTime(<time.Duration>), // defaults to 5 seconds
  memphis.MaxAckTime(<time.Duration>), // defaults to 30 sec
  memphis.MaxMsgDeliveries(<int>), // defaults to 10
  memphis.ConsumerGenUniqueSuffix(),
  memphis.ConsumerErrorHandler(func(*Consumer, error){})
)
  
// creation from a Conn
consumer1, err = c.CreateConsumer("<station-name>", "<consumer-name>", ...) 
```

### Processing Messages

First, create a callback function that receives a slice of pointers to memphis.Msg and an error.

Then, pass this callback into consumer.Consume function.

The consumer will try to fetch messages every `pullInterval` (that was given in Consumer's creation) and call the defined message handler.

```go
func handler(msgs []*memphis.Msg, err error) {
	if err != nil {
		m := msgs[0]
		fmt.Println(string(m.Data()))
		m.Ack()
	}
}

consumer.Consume(handler)
```

You can trigger a single fetch with the Fetch() method

```go
msgs, err := consumer.Fetch()
```

### Acknowledging a Message

Acknowledging a message indicates to the Memphis server to not re-send the same message again to the same consumer or consumers group.

```go
message.Ack();
```

### Get headers

Get headers per message

```
headers := msg.GetHeaders()
```

### Destroying a Consumer

```go
consumer.Destroy();
```
