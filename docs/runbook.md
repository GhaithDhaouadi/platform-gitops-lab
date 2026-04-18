# Runbook

## Prerequisites
- Docker
- Kind
- kubectl
- Helm

## Deploy Platform

### 1. Create Kind Cluster
```bash
kind create cluster --config clusters/kind/kind-config.yaml
```

### 2. Install ArgoCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl port-forward svc/argocd-server -n argocd 8080:443 &
```

### 3. Deploy Application via ArgoCD
```bash
kubectl apply -f argocd/applications/otel-demo.yaml
```

## Access Services

| Service | URL | Credentials |
|---------|-----|-------------|
| ArgoCD | http://localhost:8080 | admin / (get from secret) |
| Nginx | `kubectl port-forward -n otel-demo svc/nginx 8082:80` | No auth |
| Litmus | http://localhost:9091 | No auth |

## Chaos Testing

### Run Pod Delete Experiment
```bash
kubectl apply -f infra/litmus/pod-delete.yaml
kubectl get pods -n otel-demo -w
```

### Check Chaos Status
```bash
kubectl get chaosengine -n otel-demo
kubectl get chaosresult -n otel-demo
```

## Troubleshooting

### ArgoCD Sync Issues
```bash
kubectl get applications -n argocd
kubectl describe application otel-demo -n argocd
```

### Pods Not Running
```bash
kubectl get pods -n otel-demo
kubectl describe pod -n otel-demo <pod-name>
```

### Full Reset
```bash
kind delete cluster
kind create cluster --config clusters/kind/kind-config.yaml
kubectl apply -f argocd/applications/otel-demo.yaml
```
