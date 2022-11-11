# FAQs

### "Schema validation has failed: Invalid message format, expecting protobuf"

When producing messages to Memphis without attaching a schema, the SDK `produce` function is expecting to receive a bytes array, and therefore the standard message-producing implementation uses `Buffer.from(<string>)`

When attaching a schema, the producer is forced to pass the format that used in the schema creation, for example [protobuf](formats.md), and therefore we remove the `Buffer.from(<string>)`\
``and use (example in JS) -

```
await producer.produce({
                    message: payload,
                    // headers: headers
});
```
