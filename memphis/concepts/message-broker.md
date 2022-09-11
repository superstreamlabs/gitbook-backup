# Message broker

### A message broker is&#x20;

An architectural pattern for message validation, transformation, and routing. It mediates communication among applications, minimizing the mutual awareness that applications should have of each other in order to be able to exchange messages, effectively implementing decoupling.

### Purpose&#x20;

The primary purpose of a broker is to take incoming messages from applications and perform some action on them. Message brokers can decouple end-points, meet specific non-functional requirements, and facilitate the reuse of intermediary functions. For example, a message broker may be used to manage a workload queue or message queue for multiple receivers, providing reliable storage, guaranteed message delivery, and transaction management.

How applications communicate is becoming an increasingly large challenge. Using messaging middleware simplifies this challenge and allows for a common communications infrastructure that grows and scales to meet the most demanding conditions

### A growing list of use cases&#x20;

* Async task management
* Real-time streaming pipelines
* Data ingestion
* Async communication between services on k8s
* Queuing
* Multiple destinations to a single message
