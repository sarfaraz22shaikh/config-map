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

# Check pods are running
kubectl get pods -n mysql -l app=mysql

# Check PVCs are bound
kubectl get pvc -n mysql

# Check the headless service
kubectl get svc -n mysql

# Port-forward to test connectivity
kubectl port-forward -n mysql pod/mysql-statefulset-0 3306:3306

mysql -h 127.0.0.1 -u root -pdevops devops

kubectl delete -f statefullstate.yaml
kubectl delete -f service.yaml
kubectl delete -f configmap.yaml
kubectl delete -f kind_namespace.yaml   
