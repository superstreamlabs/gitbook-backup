---
description: >-
  What is an exchange? What are routing keys? How are exchanges and stations
  associated with each other? When should I use them and how? This page explains
  it all
---

# Data exchange

Exchange is a well-familiar concept in RabbitMQ and refers to a mechanism where a message is sent to a kind of router instead of directly to some queue. This router then directs the message to a specific queue, depending on a defined policy or, more precisely, based on a routing key.

Exchange is an excellent tool for scenarios where each message needs to be streamed to a different destination based on certain conditions or payload.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Memphis provides the functionality to route specific messages to one or several stations, depending on predefined headers.

### How it works

#### Step 1: Assign a dedicated header to each message to indicate the specific station it should be directed to:

* This can be accomplished by implementing tagging at the producer level or through a tagging function, which adds tags and headers to each message according to a highly customizable policy.

#### Step 2: Set up a station-type sink:

1. Create a station that will be designated as the "exchange" station
2. Add a Memphis station sink\


<figure><img src="../../.gitbook/assets/Screenshot 2024-01-03 at 22.25.54.png" alt=""><figcaption></figcaption></figure>

3. Two routing strategies can be selected: Header-based or Station-name-based. \
   **The Header-based strategy** involves dynamic routing for each message, determined by a specific header attached to the message. In this method, the receiving system (sink) interprets the header's value to identify the destination station and routes the message accordingly.\
   **The Station Name strategy** involves routing all new messages directly to a predetermined station.

{% hint style="info" %}
If the destination station does not exist, it will be automatically created
{% endhint %}

4. Example:\
   We implemented a station-type sink that utilizes a header-based routing strategy, keyed by "station."

```javascript
const { memphis } = require("memphis-dev");

(async function () {
    let memphisConnection

    try {
        memphisConnection = await memphis.connect({
            host: 'aws-eu-central-1.cloud.memphis.dev',
            username: '*****',
            password: '******',
            accountId: 111222333
        });

        const producer = await memphisConnection.producer({
            stationName: 'exchange',
            producerName: 'demo-producer-1'
        });
        const headers = memphis.headers()
        headers.add("station", "destination1")

        await producer.produce({
            message: { "test": "payload" }
            asyncProduce: true,
            headers: headers
        });

        memphisConnection.close();
    } catch (ex) {
        console.log(ex);
        if (memphisConnection) memphisConnection.close();
    }
})();
```

The following code will produce a message to the station `"exchange"` which will reroute it to a station called `"destination1"`

Another enhancement involves employing a custom [Memphis Function](broken-reference), which tags each message at the broker level using a unique logic tailored for every message.
