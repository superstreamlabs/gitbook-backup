---
description: Experimental
---

# Python

{% content-ref url="example-producer.md" %}
[example-producer.md](example-producer.md)
{% endcontent-ref %}

{% content-ref url="example-consumer.md" %}
[example-consumer.md](example-consumer.md)
{% endcontent-ref %}

## Installation

{% hint style="info" %}
First, install Memphis via [K8S](../../deployment/kubernetes.md) / [Docker](../../deployment/docker-compose.md).
{% endhint %}

In your project's directory:

```
pip3 install memphis-py
```

## Update

```
pip3 install --upgrade memphis-py
```

## Importing

```python
from memphis import Memphis
from memphis import retention_types, storage_types
```

```python
async def main():
  try:
    memphis = Memphis()
    await memphis.connect(
      host="<memphis-host>",
      username="<application-type username>",
      connection_token="<broker-token>",
      management_port="<management-port>", # defaults to 5555
      tcp_port="<tcp-port>", # defaults to 6666
      data_port="<data-port>", # defaults to 7766
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

Once connected, all features offered by Memphis are available.

### Disconnecting from Memphis

To disconnect from Memphis, call `Close()` on the Memphis connection object.

```python
await memphis.close()
```

### Creating a Factory

```python
factory = memphis.factory(name="<factory-name>", description="")
```

### Destroying a Factory

Destroying a factory will remove all its resources (including stations, producers, and consumers).

```python
await factory.destroy()
```

### Creating a Station

```python
station = memphis.station(
  name="<station-name>",
  factory_name="<factory-name>",
  retention_type=retention_types.MAX_MESSAGE_AGE_SECONDS, # MAX_MESSAGE_AGE_SECONDS/MESSAGES/BYTES. Defaults to MAX_MESSAGE_AGE_SECONDS
  retention_value=604800, # defaults to 604800
  storage_type=storage_types.FILE, # torage_types.FILE/torage_types.MEMORY. Defaults to MEMORY
  replicas=1, # defaults to 1
  dedup_enabled=False, # defaults to false
  dedup_window_ms: 0, # defaults to 0
)
```

### Retention Types

Memphis currently supports the following types of retention:

```python
memphis.retention_types.MAX_MESSAGE_AGE_SECONDS
```

The above means that every message persists for the value set in the retention value field (in seconds).

```python
memphis.retention_types.MESSAGES
```

The above means that after the maximum number of saved messages (set in retention value) has been reached, the oldest messages will be deleted.

```python
memphis.retention_types.BYTES
```

The above means that after maximum number of saved bytes (set in retention value) has been reached, the oldest messages will be deleted.

### Storage Types

Memphis currently supports the following types of messages storage:

```python
memphis.storage_types.FILE
```

The above means that messages persist on the file system.

```python
memphis.storage_types.MEMORY
```

The above means that messages persist on the main memory.

### Destroying a Station

Destroying a station will remove all its resources (including producers and consumers).

```python
station.destroy()
```

### Produce and Consume Messages

The most common client operations are `produce` to send messages and `consume` to receive messages.

Messages are published to a station and consumed from it by creating a consumer. Consumers are pull based and consume all the messages in a station unless you are using a consumers group, in this case messages are spread across all members in this group.

Memphis messages are payload agnostic. Payloads are `Uint8Arrays`.

In order to stop getting messages, you have to call `consumer.destroy()`. Destroy will terminate regardless of whether there are messages in flight for the client.

### Creating a Producer

```python
producer = memphis.producer(station_name="<station-name>", producer_name="<producer-name>")
```

### Producing a Message

```python
await prod.produce(
  message=bytearray(msg, 'utf-8')), # Uint8Arrays
  ack_wait_sec=15, # defaults to 15
```

### Destroying a Producer

```python
producer.destroy()
```

### Creating a Consumer

```python
consumer = memphis.consumer(
  station_name="<station-name>",
  consumer_name="<consumer-name>",
  consumer_group="<group-name>", # defaults to the consumer name
  pull_interval_ms=1000, # defaults to 1000
  batch_size=10, # defaults to 10
  batch_max_time_to_wait_ms=5000, # defaults to 5000
  max_ack_time_ms=30000, # defaults to 30000
  max_msg_deliveries=10, # defaults to 10
)
```

### Processing Messages

First, create a callback function that receives a slice of pointers to memphis.Msg.

Then, pass this callback into consumer.Consume function.

The consumer will try to fetch messages every `pull_interval_ms` (that was given in Consumer's creation) and call the defined message handler.

```python
async def msg_handler(msgs, error):
    for msg in msgs:
        print("message: ", msg.get_data())
        await msg.ack()
    if error:
        print(error)
consumer.consume(msg_handler)
 # Keep your main thread alive so the consumer will keep receiving data
await asyncio.sleep(5)
```

### Acknowledging a Message

Acknowledging a message indicates to the Memphis server to not re-send the same message again to the same consumer or consumers group.

```python
await message.ack()
```

### Destroying a Consumer

```python
consumer.destroy()
```

