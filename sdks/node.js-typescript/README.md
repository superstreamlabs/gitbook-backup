---
description: Memphis SDKs for Node.js and Typescript. Producers/Consumers examples
---

# Node.js / Typescript / NestJS

{% content-ref url="example-producer.js.md" %}
[example-producer.js.md](example-producer.js.md)
{% endcontent-ref %}

{% content-ref url="example-consumer.js.md" %}
[example-consumer.js.md](example-consumer.js.md)
{% endcontent-ref %}

{% content-ref url="example-producer.ts.md" %}
[example-producer.ts.md](example-producer.ts.md)
{% endcontent-ref %}

{% content-ref url="example-consumer.ts.md" %}
[example-consumer.ts.md](example-consumer.ts.md)
{% endcontent-ref %}

## Installation

{% hint style="info" %}
First, install Memphis via [K8S](../../deployment/kubernetes.md) / [Docker](../../deployment/docker-compose.md).
{% endhint %}

```
npm install memphis-dev
```

## Importing

For <mark style="color:green;">**javascript**</mark>, you can choose to use the `import` or `required` keyword

```
const memphis = require("memphis-dev");
```

For <mark style="color:blue;">**Typescript**</mark>, use the import keyword to aid for `typechecking` assistance

```
import memphis from 'memphis-dev';
import type { Memphis } from 'memphis-dev/types';
```

To leverage the <mark style="color:red;">**NestJS**</mark> dependency injection feature

```
import { Module } from '@nestjs/common';
import { MemphisModule, MemphisService } from 'memphis-dev/nest';
import type { Memphis } from 'memphis-dev/types';
```

### Connecting to Memphis

```
await memphis.connect({
            host: "<memphis-host>",
            port: <port>, // defaults to 6666
            username: "<username>", // (root/application type user)
            connectionToken: "<broker-token>", // you will get it on application type user creation
            reconnect: true, // defaults to true
            maxReconnect: 3, // defaults to 3
            reconnectIntervalMs: 1500, // defaults to 1500
            timeoutMs: 1500 // defaults to 1500
      });
```

#### Nest injection

```
@Module({
    imports: [MemphisModule.register()],
})

class ConsumerModule {
    constructor(private memphis: MemphisService) {}

    startConnection() {
        (async function () {
            let memphisConnection: Memphis;

            try {
               memphisConnection = await this.memphis.connect({
                    host: "<memphis-host>",
                    username: "<application type username>",
                    connectionToken: "<broker-token>",
                });
            } catch (ex) {
                console.log(ex);
                memphisConnection.close();
            }
        })();
    }
}
```

### Disconnecting from Memphis

To disconnect from Memphis, call `close()` on the Memphis object.

```
memphis.close();
```

### Creating a Station

```
const station = await memphis.station({
    name: '<station-name>',
    retentionType: memphis.retentionTypes.MAX_MESSAGE_AGE_SECONDS, // defaults to memphis.retentionTypes.MAX_MESSAGE_AGE_SECONDS
    retentionValue: 604800, // defaults to 604800
    storageType: memphis.storageTypes.FILE, // defaults to memphis.storageTypes.FILE
    replicas: 1, // defaults to 1
    dedupEnabled: false, // defaults to false
    dedupWindowMs: 0 // defaults to 0
});
```

### Retention Types

Memphis currently supports the following types of retention:

```
memphis.retentionTypes.MAX_MESSAGE_AGE_SECONDS
```

The above means that every message persists for the value set in the retention value field (in seconds).

```
memphis.retentionTypes.MESSAGES
```

The above means that after the maximum number of saved messages (set in retention value) has been reached, the oldest messages will be deleted.

```
memphis.retentionTypes.BYTES
```

The above means that after maximum number of saved bytes (set in retention value) has been reached, the oldest messages will be deleted.

### Storage Types

Memphis currently supports the following types of messages storage:

```
memphis.storageTypes.FILE
```

The above means that messages persist on the file system.

```
memphis.storageTypes.MEMORY
```

The above means that messages persist on the main memory.

### Destroying a Station

Destroying a station will remove all its resources (including producers and consumers).

```
await station.destroy();
```

### Produce and Consume Messages

The most common client operations are producing messages and consuming messages.

Messages are published to a station and consumed from it by creating a consumer. Consumers are pull-based and consume all the messages in a station unless you are using a consumers group, in which case messages are spread across all members in this group.

Memphis messages are payload agnostic. Payloads are `Uint8Arrays`.

In order to stop receiving messages, you have to call `consumer.destroy()`. The consumer will terminate regardless of whether there are messages in flight for the client.

### Creating a Producer

```
const producer = await memphisConnection.producer({
    stationName: '<station-name>',
    producerName: '<producer-name>',
    genUniqueSuffix: false
});
```

### Producing a Message

```
await producer.produce({
            message: "<bytes array>", // Uint8Arrays
            ackWaitSec: 15 // defaults to 15
});
```

### Add Header

```
const headers = memphis.headers()
headers.add('key', 'value');
await producer.produce({
    message: '<bytes array>/object', // Uint8Arrays / object in case your station is schema validated
    headers: headers // defults to empty
});
```

### Async produce

```
await producer.produce({
    message: '<bytes array>/object', // Uint8Arrays / object in case your station is schema validated
    ackWaitSec: 15, // defaults to 15
    asyncProduce: true // defaults to false
});
```

### Destroying a Producer

```
await producer.destroy();
```

### Creating a Consumer

```
const consumer = await memphisConnection.consumer({
    stationName: '<station-name>',
    consumerName: '<consumer-name>',
    consumerGroup: '<group-name>', // defaults to the consumer name.
    pullIntervalMs: 1000, // defaults to 1000
    batchSize: 10, // defaults to 10
    batchMaxTimeToWaitMs: 5000, // defaults to 5000
    maxAckTimeMs: 30000, // defaults to 30000
    maxMsgDeliveries: 10, // defaults to 10
    genUniqueSuffix: false
});
```

### To set Up connection in nestjs

```
import { MemphisServer } from 'memphis-dev/nest'

async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(
    AppModule,
    {
      strategy: new MemphisServer({
        host: '<memphis-host>',
        username: '<application type username>',
        connectionToken: '<broker-token>'
      }),
    },
  );

  await app.listen();
}
bootstrap();
```

### To Consume messages in nestjs

```
export class Controller {
    import { consumeMessage } from 'memphis-dev/nest';
    import { Message } from 'memphis-dev/types';

    @consumeMessage({
        stationName: '<station-name>',
        consumerName: '<consumer-name>',
        consumerGroup: ''
    })
    async messageHandler(message: Message) {
        console.log(message.getData().toString());
        message.ack();
    }
}
```

### Processing messages

```
consumer.on('message', (message) => {
    // processing
    console.log(message.getData());
    message.ack();
});

```

### Acknowlede a Message

Acknowledging a message indicates to the Memphis server to not re-send the same message again to the same consumer or consumers group.

```
message.ack();
```

### Get headers

Get headers per message

```
headers = message.getHeaders()
```

### Catching Async Errors

```
consumer.on("error", error => {
        // error handling
});
```

### Destroying a Consumer

```
await consumer.destroy();
```
