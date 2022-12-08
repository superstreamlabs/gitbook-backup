# Failure Scenarios

## Introduction

This section describes the exact behavior of Memphis in different failure scenarios based on three parameters:&#x20;

* Ability to read
* Ability to write
* Potential data loss

This section also aims to educate and explain Memphis's cluster mode's internals so its users can make design decisions that suit their workloads perfectly.

## Initial state

The test environment is based on a station with **three replicas** (mirrors) over **three memphis brokers** in cluster mode.

(\*) The small circle within the brokers emphasizes if they are a ["Leader" or "Follower."](concepts/station.md#leaders-and-followers)

<figure><img src="../.gitbook/assets/initial state.jpeg" alt=""><figcaption></figcaption></figure>

## Scenarios

### 1. Memphis brokers

Single broker failure, leader or follower. Produce / Consume is working usually. No data loss.

<div>

<figure><img src="../.gitbook/assets/broker 1.jpeg" alt=""><figcaption><p>Broker 2 is down</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/broker 2.jpeg" alt=""><figcaption><p>Broker 3 is down</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/broker 3.jpeg" alt=""><figcaption><p>Broker (Leader) 1 is down</p></figcaption></figure>

</div>

Two out of three replicas/brokers are down. \
Produce/Consume will be stopped until at least one replica/follower is available. No data loss.

<div>

<figure><img src="../.gitbook/assets/broker 4.jpeg" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/broker 5.jpeg" alt=""><figcaption></figcaption></figure>

</div>

The entire cluster is down.&#x20;

Produce/Consume will be stopped until at least one leader and replica/follower are available. No data loss.

<figure><img src="../.gitbook/assets/broker 6.jpeg" alt=""><figcaption></figcaption></figure>

### 2. Kubernetes Workers

The kubernetes worker that holds one of the "followers" is down.\
The cluster will wait for the broker to be "Ready."\
Produce / Consume is working usually. No data loss.

<div>

<figure><img src="../.gitbook/assets/k8s 1 (2).jpeg" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/k8s 2 (2).jpeg" alt=""><figcaption></figcaption></figure>

</div>

The kubernetes worker that holds the "leader" is down.\
The "leader" role has been taken over by broker 2.\
Producers / Consumers will might require a reconnect. No data loss.

<figure><img src="../.gitbook/assets/k8s 3.jpeg" alt=""><figcaption></figcaption></figure>

The kubernetes workers that hold the "followers"/"Leader and a follower" are down.\
No quorum for stream. No cluster leader. The station is temporarily unavailable.\
Produce/Consume will be stopped until at least one replica/follower is available. No data loss.

<div>

<figure><img src="../.gitbook/assets/k8s 4.jpeg" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/k8s 5.jpeg" alt=""><figcaption></figcaption></figure>

</div>

