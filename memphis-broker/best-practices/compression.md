# Compression

If you want to compress messages before producing them to Memphis.dev, you can manually compress the messages using a compression library like zlib or gzip and then send the compressed data to Memphis.dev. Here's a step-by-step guide on how to do this:

#### Step 1: Install the necessary libraries:

You'll need the `memphis` library for producing messages to Memphis and a compression library like `zlib` or `gzip` for message compression. You can install these libraries using `pip`:

{% code lineNumbers="true" %}
```python
from memphis import Memphis, Headers
from memphis.types import Retention, Storage
import asyncio
import zlib
```
{% endcode %}

#### Step 2: Prepare your message

Create the message that you want to send to Memphis. For example:

{% code lineNumbers="true" %}
```python
message = "This is the message you want to send to Kafka."
```
{% endcode %}

#### Step 3: Compress the message using zlib or gzip

Use the compression library to compress the message data:

{% code lineNumbers="true" %}
```python
compressed_message = zlib.compress(message.encode('utf-8'))  # Use zlib for compression
# or
# compressed_message = gzip.compress(message.encode('utf-8'))  # Use gzip for compression
```
{% endcode %}

#### Step 4: Produce the compressed message to Memphis.dev

{% code lineNumbers="true" %}
```python
await producer.produce(bytearray(compressed_message)) # you can send the message parameter as dict as well
```
{% endcode %}

#### Full example

{% code lineNumbers="true" %}
```python
from memphis import Memphis, Headers
from memphis.types import Retention, Storage
import asyncio
import zlib

async def main():
    try:
        memphis = Memphis()
        await memphis.connect(host="localhost", username="root", password="memphis")
        message = "This is the message you want to send to Kafka."
        compressed_message = zlib.compress(message.encode('utf-8'))  # Use zlib for compression
        producer = await memphis.producer(station_name="memphis-test", producer_name="producer-test")
        await producer.produce(bytearray(compressed_message)) # you can send the message parameter as dict as well

    except Exception as e:
        print(e)

    finally:
        await memphis.close()

if __name__ == '__main__':
    asyncio.run(main())
```
{% endcode %}

This code demonstrates how to manually compress messages using zlib or gzip before sending them to Memphis. When consuming messages, you would need to decompress the messages using the same compression library you used for compression before processing the data.
