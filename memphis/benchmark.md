---
description: Latest Memphis.dev benchmark reports
---

# Benchmark

## Introduction

### Benchmark Version Control

<table><thead><tr><th>Version</th><th>Date</th><th>Memphis Version</th><th>Comments</th><th data-hidden></th></tr></thead><tbody><tr><td>1.0</td><td>November 27th, 2022</td><td>0.4.1-beta</td><td>First release</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>

### Lab environment

<table><thead><tr><th>Parameter</th><th>Value</th><th data-hidden></th></tr></thead><tbody><tr><td>Cloud</td><td>AWS</td><td></td></tr><tr><td>Managed Kubernetes</td><td>EKS 1.23</td><td></td></tr><tr><td>K8S Workers</td><td>3</td><td></td></tr><tr><td>K8S Worker type</td><td><a href="https://aws.amazon.com/ec2/instance-types/">i3en.large</a></td><td></td></tr><tr><td>Memphis brokers</td><td>3</td><td></td></tr><tr><td>Memphis version</td><td>0.4.1 Cluster mode</td><td></td></tr><tr><td>Benchmark app node</td><td>m5n.8xlarge</td><td></td></tr><tr><td>Benchmark app</td><td><a href="https://github.com/memphisdev/memphis-benchmark">https://github.com/memphisdev/memphis-benchmark</a></td><td></td></tr></tbody></table>

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

## Produce (Write)

Demonstrates latency and stability over time for the different message sizes.

As can be seen, the latency starts at 0.2ms and climbs to 0.470ms - 0.549ms

<figure><img src="https://lh3.googleusercontent.com/UQW9GKGH3OCay7-zrXkKbrw9WBMOvPSJkFsZSj8h_JK2A2cxdP2I9WjRm4_uhNYkJXzRPgf5VtPQX3mtKJ4_sVxFbo3FHihG1DI1l9hkDev7yhQ6mxTtoW_1nulxtFJAlcy_VRb0p4-pLnBsEKYz2fYc4eEbmPhufU3-yJT8UJHgyeucdP-ZNw6asUgmqIko=s2048" alt=""><figcaption></figcaption></figure>

In the larger message sizes, the graph starts from 1ms to 43ms.

<figure><img src="https://lh4.googleusercontent.com/AN2AcY_fhwIFTH7cKF8TKLQSfbKiG8LtPzP6eTXL8lGbOnuWIkjuQiScGD-U0y6rTby08zkLgsBelduY6lC5_wUZzECU3irHgnZrozXcc7jIUSKOGZjz1BAdCjFZ0EumfpjUamQ0ERHvyF5SBm8-WgpF7ulH_1LZXpYCDfz1kJsD5r_BogGb0p5rpBExHSYZ=s2048" alt=""><figcaption></figcaption></figure>

## Consume (Read)

Single partition station with a single consumer.

Demonstrates latency and stability over time for different message sizes.

<figure><img src="https://lh5.googleusercontent.com/3BTmJPDkxUZ5yamsVY_0NBcmq38XMZfY0riCwecVKsYiRrxTPu-ha0-vS_n7jB-uI0kul5Y1javIv-krRged4lh2haokQbV1L8DfSyR_U4u-fTkJZ_pWSgWpPMHWt2cK8GhdA4puSB-MSMCZnTKS6-qmJuElagoE3daZRbEPW2_844m9xsSYUD6-HTeHQ1MC=s2048" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/qmKhlUfbUe7Eywyp7lcYYZnIihXeTPaGK0fJYjsFsyRAiH5U_0Uc0-Ayl6ByZfG-5FuehPq5GYznisAnPZTaqjFNXOn2JN_82L5yb0xxMWGK5G9yAeQ2GdOQVjZ2vyByuvQtOLt0n7AOIx6Lh3kd5KJ_MCeDNYK7ND0o3p4pBX3Vdpz9AtXVTPuZC56t4bZV=s2048" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/rxtjNGRNRlwEnQM9FrgsVotGQyLZXg0nXsj44y72dkHXtalhf8aLh2rGqupVAtVEAXvXYQXlHVkqV5YXZuy1TCnzRzS-g-DMDr-eeOMPkBBRqpkVbeuMb3CvFdyd2Bupoul5fGL8QFtBqXQBNrKUOlZUKYkSuoj6krCs0TGTj4nbH4M8entvyWDrnljXnTPa=s2048" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh4.googleusercontent.com/HZVkwhev0OUC-lOB-KR7iBvNxUtHkFF3M2dF_Fbc5lxbpud3WUsX0SLUGXf2TToh8mYPoqoH8vwBSqkPFRE8FVcNDPftt6g8-AX_0Qnp_rrL91zTG6W8EGFTMsYSJOUNq55PcDJQu9U5aBf9z24DuvX6rp5m1iyHiYiBu5hKbJroD_M5RHxB6AfkLiiuNfol=s2048" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh4.googleusercontent.com/P45AesUOcfO3gWI2Qsa2M4HuyFIk1VuAjqKZij9hEqHqa9xxY-wP_sGu914jStkwU6e6COh40bVC1oU-oJ5z5Be5emVSdrDcnQgyBgMFvfveuXyNtBo-yw1FDeT3aCDAS5_6HTvlGkWi5lav3-h45oKMRaIAKYUhMQWAxB8QE6DPiuxFKAW3087I1aas-E1X=s2048" alt=""><figcaption></figcaption></figure>

## Stress test (1KB message size)

This test demonstrates the behavior of Memphis under the constant stress of producing and consuming messages simultaneously.

<figure><img src="https://lh6.googleusercontent.com/xurwFrTZX67M8Fc17kpIdVFFwyekFIDB7RV4ctFWkAEX_aYHk8WVtpOjXcGPkUBzdBU04Hg-bnOJanF9y4s2wCtz7JgV500XDzgaqvZk5i_MsMsYVhqVzvYnIt0sRZNzHQO11_07oCWnuJ9eA0SH7V6yg7kI_W6_vSkJ-13eBpNp-NWOfEpCxd7jeqEHMf2T=s2048" alt=""><figcaption></figcaption></figure>
