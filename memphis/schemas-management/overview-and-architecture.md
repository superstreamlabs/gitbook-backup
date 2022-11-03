# Overview and Architecture

In a federated data platform, which responsibilities are distributed between stakeholders, teams, and sources, it’s harder to control establish a single standard. This is where the data contracts concept comes into play. Why do data contracts matter? Because (a) they provide insights into who owns what data products. (b) they support setting standards and managing your data pipelines with confidence. They also provide crucial information on what data is being consumed, by whom, and for what purpose. Bottom line: data contracts are essential for robust data management!

<figure><img src="../../.gitbook/assets/schema 1.jpeg" alt=""><figcaption></figcaption></figure>

The very basic building block to control and ensure the quality of data that flows through your organization between the different owners is by defining well-written schemas and data models.

#### Why?

Data pipelines are constantly breaking and creating data quality AND usability issues. There is a communication chasm between service implementers, data engineers, and data consumers. There are multiple approaches to solving these issues and data engineers are still very much pioneers exploring the frontier of future best practices.

#### Popular formats&#x20;

A quick overview of the most popular formats&#x20;

* Protobuf. \
  The modern. Protocol buffers are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data – think XML, but smaller, faster, and simpler.&#x20;
* Avro. \
  The popular. Apache Avro™ is the leading serialization format for record data, and first choice for streaming data pipelines. It offers excellent schema evolution.&#x20;
* JSON \
  The simplest. JSON Schema is a vocabulary that allows you to annotate and validate JSON documents.

### Memphis Schema Verse Architecture

<figure><img src="https://lh6.googleusercontent.com/VtydcT2Ajl-JV_CF404Mk6gLeWNVVwceIhvkLGlANrBltLZWuzJOFArB9XmXMKwcI8En_kBI3V3ocSCbk2WDUwg5vbiC53VN-gWEFmOdL6zv3u4J8ASRgSV-0rCCo1cQQ_0HeyBbbi6Jo0d1FJIZ-Ew0krRoZ-HIY6f1XijA9uBOkpZe9tfmovfJHwBGuA" alt=""><figcaption></figcaption></figure>

Memphis Schema Verse provides a robust schema store and schema management layer on top of memphis broker without a standalone compute or dedicated resources. With a unique and modern UI and programmatic approach, both technical and non-technical users can create and define different types of schemas, attach the schema to multiple stations and choose if the schema should be enforced or not. Memphis' low-code approach removes the serialization part as it is embedded within the producer library. Schema X supports versioning, GitOps methodologies, and schema evolution.

Also includes -&#x20;

* Great UI and programmatic approach Embed within the broker&#x20;
* Zero-trust enforcement&#x20;
* Versioning&#x20;
* Out-of-the-box monitoring&#x20;
* Import & Export schemas
* Low/no-code validation and serialization&#x20;
* No configuration needed
* Native support in Python, Go, Node.js

#### Serialization&#x20;

process Serialization is converting a data object—a combination of code and data—into a series of bytes that saves the object's state in an easily transmittable form. The opposite process is called deserialization. Serialization is basically represented as a function, each data format with its own implementation, and in case the structure and content of a given data do not match the defined schema in the .proto/.avro/.json struct, the serialization process fails, and therefore, the message will not be sent to the broker. That process creates the initial validation of the message before it reaches the broker itself, using the client cache to store the schema locally.

<figure><img src="https://lh5.googleusercontent.com/9ifhev7freLnIYyD_Y3zmrgZAp9-2Bf8eYsSAps0N_77PblO4eG0LGodJY6C6bBmhCxYDRMocztYK3Sge8WMezMMrZFyODEBOw5YZ2xmB7xqqrkhJcds-f67XqHSXNTydr3PpcI2e09yze32L4h0_kg3CcZAxPepTFtJJ_oStF-myZdomFjy2t7XVxZf" alt=""><figcaption></figcaption></figure>
