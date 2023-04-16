---
description: >-
  This section describes the best practices to run Memphis in Production
  environment and to maximize
---

# Production Best Practices

1. Use Memphis in cluster mode to enable parallel usage across multiple brokers
2. Spread the workloads across as many stations as possible (if possible) to spread leaders across different brokers
3. Use memory as the primary storage type
4. Use a low amount of replicas or none
5. Stretch the retention to days/hours
6. Make sure to utilize as many cores as possible within the app itself. For example, in node.js - use threads and cluster
7. Use affinity rules and separate the client K8s worker from the memphis workers
8. The most recommended K8s hardware would be arm-based CPUs with at least 2 cores (for example, AWS Graviton) and at least 8GB of memory. Again, not mandatory, but definitely improves performance
