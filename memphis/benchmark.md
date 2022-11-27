---
description: Full workload, latency, and performance report
---

# Benchmark

We're excited to announce the release of our 1st benchmark report.&#x20;

Memphis has been under heavy development for the past two years, and we've reached a state where we can display some actual performance numbers.&#x20;

## Introduction

### Benchmark Version Control

<table><thead><tr><th>Version</th><th>Date</th><th>Memphis Version</th><th>Comments</th><th data-hidden></th></tr></thead><tbody><tr><td>1.0</td><td>November 27th, 2022</td><td>0.4.0-beta</td><td>First release</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>

### Lab environment

<table><thead><tr><th>Parameter</th><th>Value</th><th data-hidden></th></tr></thead><tbody><tr><td>Cloud</td><td>AWS</td><td></td></tr><tr><td>Managed Kubernetes</td><td>EKS 1.23</td><td></td></tr><tr><td>K8S Workers</td><td>3</td><td></td></tr><tr><td>K8S Worker type</td><td><a href="https://aws.amazon.com/ec2/instance-types/">i3en.large</a></td><td></td></tr><tr><td>Memphis brokers</td><td>3</td><td></td></tr><tr><td>Memphis version</td><td>0.4.0 Cluster mode</td><td></td></tr><tr><td>Benchmark app node</td><td>m5n.8xlarge</td><td></td></tr></tbody></table>

### Test cases

* Message size -
  * 512B
  * 1KB
  * 256KB
  * 1MB
* Messages rate - 300msgs/sec\*
* Method: Async

(\*) To align and run the same frequency across the different message sizes

### Notes

1. In each iteration, the counter starts with the first sent message, stops at the last one, waits until the end of the second, and repeats.
2. Each test runs 1000 times to produce “normalized” results.

\---------------------------------------------------

## Produce

Demonstrates latency and stability over time for the different message sizes.

As can be seen, the graph starts at 0.2ms and climbs to 0.470ms - 0.549ms

<figure><img src="../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

In the larger sizes, the graph starts from 26.47ms-435ms and climbs to 65.229ms-503ms

<figure><img src="https://lh5.googleusercontent.com/qYebZ51rxeKnPoriPCJkQ1lf-U5J-1boqNk8bV49SuNitzL2vNB49tHCevtWOz0yxB__tumnE_sNaMq7f528YsnKmn52P1UpXV3v2ZabfykA2DShkiuwnRtDYkmiCW0KqaEipz_IOOpNizW-PqEaC4F4Ui2xJXUd2DnVchkstaB9k-oB2dBpjUlAf10KRw" alt=""><figcaption></figcaption></figure>

Summary table

| Size                                         | 99        | 99.9      | 99.99     | 99.999    |
| -------------------------------------------- | --------- | --------- | --------- | --------- |
| <mark style="color:purple;">**512B**</mark>  | 0.411ms   | 0.505ms   | 0.545ms   | 0.549ms   |
| <mark style="color:purple;">**1KB**</mark>   | 0.388ms   | 0.456ms   | 0.469ms   | 0.470ms   |
| <mark style="color:purple;">**256KB**</mark> | 26.473ms  | 58.554ms  | 64.622ms  | 65.229ms  |
| <mark style="color:purple;">**1MB**</mark>   | 435.940ms | 502.651ms | 502.983ms | 503.016ms |

The following graph demonstrates heavy-workload jobs, and the time the broker takes to finish a typical 1KB message size.

<figure><img src="https://lh4.googleusercontent.com/JR3E-N-GhD8vqmfpsWXc3BhJUKeU2bwwDrDYu-D_A2EM2dfWG_W3BugbxHDUjID60axdvAt30cfm6immlZbdOvh-V1Cj5n3P31p_dZV8D5NxpUPrpt4CbVcwq2wbWh58oriaFZGDo92GoEwWjfZGaZ7DQDmBQb6A6PGwVWVYlCpqNfcM2YqdaCzJkBjS9A" alt=""><figcaption></figcaption></figure>

The following graph demonstrates the maximum amount of messages per second, per size.

<figure><img src="https://lh5.googleusercontent.com/klva3hM0BCjJt1KJasKIN78WlB22OJOP3BQFqvwY8RwCZj3HlERhYC6VW3o5sDZTQeWgAeLxdlYlYhEOzw-jIsB1IN0pkm9h0j2gCxrU08P90RdIMa7ZaozoLyq4jRpQwFF9nXyXNZyYMKpPPkNAGidHPGbWkrcYppYAHvzeW3ElUkWSevvX9WvKouybSg" alt=""><figcaption></figcaption></figure>

The following graph demonstrates the average latency across 1000 seconds per size. As can be seen, the general trend is fairly steady across the charts and through time.

<figure><img src="https://lh5.googleusercontent.com/vdYsTRbkiBr7bHQUCR2nEDvY_BlvW7WkGCrgI7VtPfp22TjjQpjwsJiwr9awCHbMLY_gXJ7F_B1szq7WtZcbBvJemBXwWC7EVq9n9Hl3PDqXjyoC3ykCXwC1bqOPqCK0jlz_sQEzXZ-TLi7oDym8aHlPRU_Fp1q0ERvV5ZJBs7BnlFjwTTAs0kbH4fuL8Q" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/_r3lQgvhLo_ij9_HtWPyAIKOk531vm9dF8hiwZe1aGsG-rWX9uIANIdFxLMvq_K4Lhdph_cCHnL1LR9u1ST8zoFwDWws0q4UcYhjcCLe9WA1jK3Fs064P3TfTwNl8NKMkfBh2aF4sdGRWrWLk95aycseNnlOCyaZiExUYg8IE50LZCz4ipXYHoNw5K_BhA" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh3.googleusercontent.com/8vjaKSy68iGYaOOhKf5JvDpdxai316awkZ3tdiybnGJBcK82-EkoaleP8eOhj1IZOc8jGQVY4TQHP6G_zCe_fjTNJbmf3LD1bDexdiltNE5xnOGcyB2-yj6YUTfISiHslb-D7QY6MWwmbeVxsVM6rhZzXU_RkWTgAXSwl8ZzfBdqHTUymE2G-LT2QXTJ_g" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh4.googleusercontent.com/etazDswFwwpJjJc-qeqVLV2_SFPc6dTqE7DT6VwdSOD66N2UnUJud9wbLkOj-2YF6vuSLIY_P9pkrEGZk-d_C_MGf8McCFQDc3Ig1B0oOX3QJ9YBsk80zXyF8nCI_r0D-8DPSRYPYfh5naeh7K-zva_9IUEVBj8JMox2u1hzbhGH2Ru9YVmiuxK2MOoFxA" alt=""><figcaption></figcaption></figure>

