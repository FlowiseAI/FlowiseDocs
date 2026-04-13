---
description: Learn how to deploy Flowise on Kubernetes using Helm
---

# Kubernetes

***

Deploy Flowise on Kubernetes using community-maintained Helm charts.

## HelmForge Chart

[HelmForge](https://helmforge.dev) provides a Helm chart that supports both standalone and queue deployment modes.

* Chart source: [HelmForge Charts on GitHub](https://github.com/helmforgedev/charts/tree/main/charts/flowise)
* ArtifactHub: [Flowise chart on ArtifactHub](https://artifacthub.io/packages/helm/helmforge/flowise)

### Prerequisites

* A running Kubernetes cluster (v1.26+)
* [Helm](https://helm.sh/docs/intro/install/) v3 installed

### Install

```bash
helm repo add helmforge https://repo.helmforge.dev
helm repo update
helm install flowise helmforge/flowise
```

Or install directly from the OCI registry:

```bash
helm install flowise oci://ghcr.io/helmforgedev/helm/flowise
```

### Standalone Mode (default)

The default installation runs Flowise as a single Deployment with SQLite and local storage. This is suitable for evaluation and small workloads.

### Queue Mode

For scalable production deployments, enable queue mode with separate main and worker pods backed by Redis and PostgreSQL:

```bash
helm install flowise helmforge/flowise \
  --set architecture=queue \
  --set postgresql.enabled=true \
  --set redis.enabled=true
```

Queue mode deploys:

* **Main pod** — serves the web UI and API
* **Worker pods** — process AI execution tasks from the Redis queue
* **PostgreSQL** — persistent database (bundled subchart or external)
* **Redis** — message queue (bundled subchart or external)

### Ingress

Enable ingress to expose the Flowise UI:

```yaml
# values.yaml
ingress:
  enabled: true
  ingressClassName: nginx
  hosts:
    - host: flowise.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: flowise-tls
      hosts:
        - flowise.example.com
```

```bash
helm upgrade --install flowise helmforge/flowise -f values.yaml
```

For full configuration options and architecture details, see the [chart documentation](https://helmforge.dev/docs/charts/flowise).

## Cowboysysop Chart

An alternative community chart is available from [cowboysysop](https://artifacthub.io/packages/helm/cowboysysop/flowise).

```bash
helm repo add cowboysysop https://cowboysysop.github.io/charts/
helm install flowise cowboysysop/flowise
```
