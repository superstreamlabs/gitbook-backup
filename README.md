---
cover: .gitbook/assets/Your_streaming_journey_starts_here..jpg
coverY: 0
---

# Step 1 - Installation

### What is Memphis{dev}?

Memphis{dev} is an open-source real-time data processing platform that provides end-to-end support for in-app streaming use cases using Memphis distributed message broker.&#x20;

Memphis{dev} enables building next-generation applications that require large volumes of streamed and enriched data, modern protocols, zero ops, rapid development, extreme cost reduction, and a significantly lower amount of dev time for data-oriented developers and data engineers.

**Memphis focuses on four pillars**

1. Performance - Enhanced cache usage
2. Resiliency - Never lose a message and 99.95% uptime
3. Observability - Out-of-the-box observability that makes sense and reduces troubleshooting time
4. Developer Experience - Rapid Development, Modularity, inline processing, Schema management

### 👉 **Start here: Choose your preferred environment -**&#x20;

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden></th><th data-hidden></th><th data-hidden data-type="files"></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Kubernetes (Production)</strong></td><td><a href="./#undefined">Here</a></td><td>Kubernetes (Production)</td><td></td><td><a href=".gitbook/assets/kubernetes banner (1).jpeg">kubernetes banner (1).jpeg</a></td><td><a href=".gitbook/assets/kubernetes banner (2).jpeg">kubernetes banner (2).jpeg</a></td><td></td></tr><tr><td><strong>Cloud Providers (Prod)</strong></td><td><a href="deployment/cloud-deployment/">Here</a></td><td></td><td></td><td></td><td><a href=".gitbook/assets/cloud providors banner (1).jpeg">cloud providors banner (1).jpeg</a></td><td></td></tr><tr><td><strong>Docker (Dev)</strong></td><td><a href="./#undefined">Here</a></td><td></td><td></td><td></td><td><a href=".gitbook/assets/Docker banner.jpeg">Docker banner.jpeg</a></td><td></td></tr></tbody></table>

### Kubernetes (Production)

```
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update && helm install memphis memphis/memphis --create-namespace --namespace memphis --wait
```

For more information please hear [here.](deployment/kubernetes.md)

### Docker (Dev)

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```

For more information please head [here.](deployment/docker-compose.md)
