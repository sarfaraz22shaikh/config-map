# MySQL StatefulSet on Kubernetes

This project deploys a **3-replica MySQL 8.0 StatefulSet** on a Kubernetes cluster using a headless Service, ConfigMap, and persistent volume claims.

## Architecture

| Resource | File | Purpose |
|----------|------|---------|
| Namespace | `kind_namespace.yaml` | Creates the `mysql` namespace |
| ConfigMap | `configmap.yaml` | Stores `MYSQL_DATABASE` value |
| Headless Service | `service.yaml` | Provides stable network identity for StatefulSet pods |
| StatefulSet | `statefullstate.yaml` | Deploys 3 MySQL replicas with persistent storage |

## Prerequisites

- Kubernetes cluster (minikube, kind, k3s, etc.)
- `kubectl` configured and pointing to the cluster
- A default StorageClass available in the cluster

## Deployment

Apply resources in the following order:

```bash
kubectl apply -f kind_namespace.yaml
kubectl apply -f configmap.yaml
kubectl apply -f service.yaml
kubectl apply -f statefullstate.yaml   
