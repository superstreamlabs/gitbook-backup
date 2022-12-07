---
description: Full workload, latency, and performance report
---

# Benchmark

We're excited to announce the release of our 1st benchmark report.&#x20;

Memphis has been under heavy development for the past two years, and we've reached a state where we can display some actual performance numbers.&#x20;

## Introduction

### Benchmark Version Control

<table><thead><tr><th>Version</th><th>Date</th><th>Memphis Version</th><th>Comments</th><th data-hidden></th></tr></thead><tbody><tr><td>1.0</td><td>November 27th, 2022</td><td>0.4.1-beta</td><td>First release</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>

### Lab environment

<table><thead><tr><th>Parameter</th><th>Value</th><th data-hidden></th></tr></thead><tbody><tr><td>Cloud</td><td>AWS</td><td></td></tr><tr><td>Managed Kubernetes</td><td>EKS 1.23</td><td></td></tr><tr><td>K8S Workers</td><td>3</td><td></td></tr><tr><td>K8S Worker type</td><td><a href="https://aws.amazon.com/ec2/instance-types/">i3en.large</a></td><td></td></tr><tr><td>Memphis brokers</td><td>3</td><td></td></tr><tr><td>Memphis version</td><td>0.4.1 Cluster mode</td><td></td></tr><tr><td>Benchmark app node</td><td>m5n.8xlarge</td><td></td></tr></tbody></table>

### Test cases

* Message size -
  * 512B
  * 1KB
  * 256KB
  * 1MB
* Messages rate - 300msgs/sec\*

(\*) To align and run the same frequency across the different message sizes

### Notes

1. In each iteration, the counter starts with the first sent message, stops at the last one, waits until the end of the second, and repeats.
2. Each test runs 1000 times to produce “normalized” results.

\---------------------------------------------------

## Produce

Method: Async.

Demonstrates latency and stability over time for the different message sizes.

As can be seen, the graph starts at 0.2ms and climbs to 0.470ms - 0.549ms

<figure><img src="../.gitbook/assets/Produce - Latency (512B-1KB).jpeg" alt=""><figcaption></figcaption></figure>

In the larger sizes, the graph starts from 26.47ms-435ms and climbs to 65.229ms-503ms

<figure><img src="../.gitbook/assets/Produce - Latency (256KB-1MB).jpeg" alt=""><figcaption></figcaption></figure>

Summary table

| Size                                         | 99        | 99.9      | 99.99     | 99.999    |
| -------------------------------------------- | --------- | --------- | --------- | --------- |
| <mark style="color:purple;">**512B**</mark>  | 0.411ms   | 0.505ms   | 0.545ms   | 0.549ms   |
| <mark style="color:purple;">**1KB**</mark>   | 0.388ms   | 0.456ms   | 0.469ms   | 0.470ms   |
| <mark style="color:purple;">**256KB**</mark> | 26.473ms  | 58.554ms  | 64.622ms  | 65.229ms  |
| <mark style="color:purple;">**1MB**</mark>   | 435.940ms | 502.651ms | 502.983ms | 503.016ms |

The following graph demonstrates heavy-workload jobs, and the time the broker takes to finish a typical 1KB message size.

<figure><img src="../.gitbook/assets/heavy-workload jobs.jpeg" alt=""><figcaption></figcaption></figure>

The following graph demonstrates the maximum amount of messages per second, per size.

<figure><img src="../.gitbook/assets/sec vs. MsgSize 2.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/sec vs. MsgSize.jpg" alt=""><figcaption></figcaption></figure>

The following graph demonstrates the average latency across 1000 seconds per size. As can be seen, the general trend is relatively steady across the charts and through time.

<figure><img src="../.gitbook/assets/Average latency(ms) - 512B.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Average latency(ms) - 1KB.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Average latency(ms) - 256KB.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Average latency(ms) - 1MB (1).jpeg" alt=""><figcaption></figcaption></figure>

## Consume

Single partition station with a single consumer.

Demonstrates latency and stability over time for the different message sizes.

<figure><img src="../.gitbook/assets/Consume - Latency.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Consume - Latency 1mb.jpeg" alt=""><figcaption></figcaption></figure>

| Size                                         | 99        | 99.9      | 99.99     | 99.999    |
| -------------------------------------------- | --------- | --------- | --------- | --------- |
| <mark style="color:purple;">**512B**</mark>  | 62.362ms  | 67.355ms  | 69.542ms  | 69.350ms  |
| <mark style="color:purple;">**1KB**</mark>   | 62.061ms  | 67.219ms  | 69.156ms  | 69.350ms  |
| <mark style="color:purple;">**256KB**</mark> | 407.512ms | 443.794ms | 482.947ms | 486.947ms |
| <mark style="color:purple;">**1MB**</mark>   | 1s        | 1s        | 1s        | 1s        |

The following graph demonstrates the maximum amount of consumed messages per second, per size.

<figure><img src="https://lh5.googleusercontent.com/Lxx96CK6iO8c3mTpNL3TO9F2QPZTQrYPq3QNpghvukrlNP2nRB-fKEsEFPNHLedN6y-NAX5TvIlgsbVpVXEnLeNPHJ-eWQdlFcn143yZXSpI005usMsRe6XN-AHrKpqK0Rhm8NK7cInOww7RmFmBCPzl78vZae5OrJr8xXW2TlohSSBBw9t39AnluzkRmw" alt=""><figcaption></figcaption></figure>

Average latency over 1000 seconds

<figure><img src="https://lh5.googleusercontent.com/A4IchAq0lnKdU4pD-hAtT9Q1DAE2JUxbM52CrkGM2hNys8lvLGnLI5MEFjS2pxDSyDBRwxjkkE5Ph9Ajt9JhTgXElxy2r9l9pImLo2_aqRjAhfkx4ojK01D9P6iZ7bmnmhxpwWeEKxTioukvZ5FwmJcZAe109U0dFWT4b48CN2MCtY3UBj8rLWMe2ODqQA" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh4.googleusercontent.com/PgobweDTsP49kNAtaPT4qi6i7MaG8dBiHhwPFPfmjR5bErdaHkoQI1akWi0QEaAaO2HJz-QmesKnW1_6YRRzsDkWOi7skftELPUEq3bc9tZFO3BlEhY_EY6AYjvCCvXfySmhb2Wxc-9a8DPf0UUkgZLBJzDf3MLMHbSEMOy2sKWkJWaJpnMzUUlbqszpKw" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh3.googleusercontent.com/RmdQU1hBdBMfKxl6RWl7PHVdTX7k2nQwcdcHK79I9gofw5W8AAWqHPb1li6BpyUYkUcqpSTAQUvAt54DZSYRGkxI1m3jiBarKAwbjAjtQQsY-ZToZrgH6pKNzi-WevWcsYl3Pn2Nt_N_LKcngXIjCV9NVORu3dIJSvK6nO-HIjFnOOMUDmwr5AjfacNBbw" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/zRFOzvM-5QQ2w55lbQcgYVIgj-07go8oohaGaSQru7utgVxDSFPj5CkM0ZbZnrXbprJWPoC4Ns0c3O8qYzKv3Xd5nkJ1UHWGc99Uyfbz2onVS8zZWdHN-fbxFM3P_aI0Of1_Gjid0h43pdXnaIFY1vGmkQ_otx8zaPwY0f7NqRQSTdRtaEemjSbvESrjYg" alt=""><figcaption></figcaption></figure>
