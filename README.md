# Platform GitOps Lab

> Self-Healing GitOps Platform with Chaos Engineering

## 🎯 Project Overview

This project demonstrates a complete GitOps workflow using ArgoCD on Kind Kubernetes, with integrated chaos engineering using Litmus.

## 🏗️ Architecture

```
GitHub → ArgoCD → Kind Cluster → Nginx App
                       ↓
                  Litmus Chaos
```

## 📋 Prerequisites

- Docker
- Kind (Kubernetes in Docker)
- kubectl
- Helm

## 🚀 Quick Start

```bash
# 1. Create cluster
kind create cluster --config clusters/kind/kind-config.yaml

# 2. Install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 3. Deploy application
kubectl apply -f argocd/applications/otel-demo.yaml
```

## 🌐 Access Services

| Service | URL | Credentials |
|---------|-----|-------------|
| ArgoCD | http://localhost:8080 | admin / (get from secret) |
| Nginx | http://localhost:8082 | No auth |
| Litmus | http://localhost:9091 | No auth |

## 🔥 Chaos Engineering

```bash
# Run chaos experiment
kubectl apply -f infra/litmus/pod-delete.yaml

# Watch pods restart
kubectl get pods -n otel-demo -w
```

## 📁 Project Structure

```
platform-gitops-lab/
├── apps/otel-demo/base/     # Kustomize base manifests
├── argocd/applications/     # ArgoCD app definitions
├── clusters/kind/           # Kind cluster config
├── infra/litmus/            # Chaos experiments
├── docs/                    # Documentation
└── .github/workflows/       # CI/CD pipelines
```

## 🛠️ Technologies

- **Kubernetes**: Kind (local cluster)
- **GitOps**: ArgoCD
- **Application**: Nginx
- **Chaos Engineering**: Litmus
- **CI/CD**: GitHub Actions
