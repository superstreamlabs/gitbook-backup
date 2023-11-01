---
cover: ../../.gitbook/assets/Memphis concepts (2).jpeg
coverY: 0
---

# Ordering

Memphis.dev provides a strong guarantee of ordering and delivery, ensuring that messages are delivered reliably and in the order in which they were produced. This guarantee is particularly important when working with a single consumer or within a single consumer group, where message order is preserved.&#x20;

In scenarios involving multiple consumer groups, the guarantee of ordering might not hold, as Memphis.dev focuses on delivering high throughput and low latency, potentially sacrificing strict ordering for improved performance. This flexibility allows developers to choose the right trade-off between performance and ordering based on their specific application requirements.

![](../../.gitbook/assets/ordering.jpeg)
