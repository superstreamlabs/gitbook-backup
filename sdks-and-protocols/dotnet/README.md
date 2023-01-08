---
description: Experimental
---

# .NET

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

```shell
dotnet add package Memphis.Client -v ${MEMPHIS_CLIENT_VERSION}
```

## Update

```powershell
Update-Package Memphis.Client
```

## Importing

```csharp
using Memphis.Client;
```

## Connecting to Memphis

First, we need to create or use default `ClientOptions` and then connect with Memphis by using `MemphisClientFactory.CreateClient(ClientOptions opst)`.

```csharp
try
{
    var options = MemphisClientFactory.GetDefaultOptions();
    options.Host = "<broker-address>";
    options.Username = "<application-type-username>";
    options.ConnectionToken = "<token>";
    var memphisClient = MemphisClientFactory.CreateClient(options);
    ...
}
catch (Exception ex)
{
    Console.Error.WriteLine("Exception: " + ex.Message);
    Console.Error.WriteLine(ex);
}
```

Once connected, all features offered by Memphis are available.

### Disconnecting from Memphis

To disconnect from Memphis, call `Dispose()` on the `MemphisClient`.

```csharp
await memphisClient.Dispose()
```

## Creating a Station

```csharp
try
{
    // First: creating Memphis client
    var options = MemphisClientFactory.GetDefaultOptions();
    options.Host = "<memphis-host>";
    options.Username = "<application type username>";
    options.ConnectionToken = "<broker-token>";
    var client = MemphisClientFactory.CreateClient(options);
    
    // Second: creaing Memphis station
    var station = await client.CreateStation(
        stationOptions: new StationOptions()
        {
            Name = "<station-name>",
            RetentionType = RetentionTypes.MAX_MESSAGE_AGE_SECONDS,
            RetentionValue = 604_800,
            StorageType = StorageTypes.DISK,
            Replicas = 1,
            IdempotencyWindowMs = 0,
            SendPoisonMessageToDls = true,
            SendSchemaFailedMessageToDls = true,
        });
}
catch (Exception ex)
{
    Console.Error.WriteLine("Exception: " + ex.Message);
    Console.Error.WriteLine(ex);
}
```

### Retention Types

Memphis currently supports the following types of retention:

```csharp
RetentionTypes.MAX_MESSAGE_AGE_SECONDS
```

The above means that every message persists for the value set in the retention value field (in seconds).

```csharp
RetentionTypes.MESSAGES
```

The above means that after the maximum number of saved messages (set in retention value) has been reached, the oldest messages will be deleted.

```csharp
RetentionTypes.BYTES
```

The above means that after maximum number of saved bytes (set in retention value) has been reached, the oldest messages will be deleted.

### Storage Types

Memphis currently supports the following types of messages storage:

```python
StorageTypes.DISK
```

The above means that messages persist on disk.

```python
StorageTypes.MEMORY
```

The above means that messages persist on the main memory.

### Destroying a Station

Destroying a station will remove all its resources (including producers and consumers).

```csharp
station.DestroyAsync()
```

### Produce and Consume Messages

The most common client operations are `produce` to send messages and `consume` to receive messages.

Messages are published to a station and consumed from it by creating a consumer. Consumers are pull based and consume all the messages in a station unless you are using a consumers group, in this case messages are spread across all members in this group.

Memphis messages are payload agnostic. Payloads are `byte[]`.

In order to stop getting messages, you have to call `consumer.Dispose()`. Destroy will terminate regardless of whether there are messages in flight for the client.

### Creating a Producer

```csharp
try
{
   // First: creating Memphis client
    var options = MemphisClientFactory.GetDefaultOptions();
    options.Host = "<memphis-host>";
    options.Username = "<application type username>";
    options.ConnectionToken = "<broker-token>";
    var client = MemphisClientFactory.CreateClient(options);

    // Second: creating the Memphis producer 
    var producer = await client.CreateProducer(
        stationName: "<memphis-station-name>",
        producerName: "<memphis-producer-name>",
        generateRandomSuffix:true);    
}
catch (Exception ex)
{
    Console.Error.WriteLine("Exception: " + ex.Message);
    Console.Error.WriteLine(ex);
}
```

## Producing a message

```python
var commonHeaders = new NameValueCollection();
commonHeaders.Add("key-1", "value-1");

await producer.ProduceAsync(Encoding.UTF8.GetBytes(text), commonHeaders);
```

### Destroying a Producer

```csharp
await producer.DestroyAsync()
```

### Creating a Consumer

```csharp
try
{
    // First: creating Memphis client
    var options = MemphisClientFactory.GetDefaultOptions();
    options.Host = "<memphis-host>";
    options.Username = "<application type username>";
    options.ConnectionToken = "<broker-token>";
    var client = MemphisClientFactory.CreateClient(options);
    
    // Second: creaing Memphis consumer
    var consumer = await client.CreateConsumer(new ConsumerOptions
    {
        StationName = "<station-name>",
        ConsumerName = "<consumer-name>",
        ConsumerGroup = "<consumer-group-name>",
    }); 
       
}
catch (Exception ex)
{
    Console.Error.WriteLine("Exception: " + ex.Message);
    Console.Error.WriteLine(ex);
}
```

### Create message handler for consuming Messages&#x20;

First, create a callback functions that receives a args that holds list of MemhpisMessage.

Then, pass this callback into consumer.Consume function.

The consumer will try to fetch messages every `PullIntervalMs` (that was given in Consumer's creation) and call the defined message handler.

```csharp
EventHandler<MemphisMessageHandlerEventArgs> msgHandler = (sender, args) =>
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
};
```

## Consuming messages

```csharp
await consumer.ConsumeAsync( msgCallbackHandler:msgHandler, dlqCallbackHandler:msgHandler);
```

### Acknowledging a Message

Acknowledging a message indicates to the Memphis server to not re-send the same message again to the same consumer or consumers group.

```csharp
msg.Ack();
```

### Get headers

Get headers per message

```csharp
msg.GetHeaders()
```

### Destroying a Consumer

```csharp
await consumer.DestroyAsync();
```

## [Schemaverse](./#schemaverse)

### Attaching a Schema to an Existing Station

```csharp
await client.AttachSchema(stationName: "<station-name>", schemaName: "<schema-name>");
```

### Detaching a Schema from Station

```
await client.DetachSchema(stationName: station.Name);
```
