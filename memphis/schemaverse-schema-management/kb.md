# KB

### "Schema validation has failed: Invalid message format, expecting protobuf"

When producing messages to Memphis without attaching a schema, the SDK `produce` function is expecting to receive a bytes array, and therefore the standard message-producing implementation uses `Buffer.from(<string>)`

When attaching a schema, the producer is forced to pass the format that used in the schema creation, for example [protobuf](formats/), and therefore we remove the `Buffer.from(<string>)`\
and use (example in JS) -

```
await producer.produce({
    message: payload,
    headers: headers
});
```

### Release v0.4.0 - For protobuf, consumers are required to self-decode consumed messages with a self-hosted .proto file

{% code lineNumbers="true" %}
```javascript
const memphis = require("memphis-dev");
var protobuf = require("protobufjs");

(async function () {
    try {
        await memphis.connect({
            host: "localhost",
            username: "root",
            connectionToken: "memphis"
        });

        const consumer = await memphis.consumer({
            stationName: "marketing",
            consumerName: "cons1",
            consumerGroup: "cg_cons1",
            maxMsgDeliveries: 3,
            maxAckTimeMs: 2000,
            genUniqueSuffix: true
        });

        const root = await protobuf.load("schema.proto");
        var TestMessage = root.lookupType("Test");

        consumer.on("message", message => {
            const x = message.getData()
            var msg = TestMessage.decode(x);
            console.log(msg)
            message.ack();
        });
        consumer.on("error", error => {
            console.log(error);
        });
    } catch (ex) {
        console.log(ex);
        memphis.close();
    }
})();
```
{% endcode %}
