# Architecture Diagram

## Components
┌─────────────────────────────────────────────────────────────┐
│ GitHub Repository │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ apps/otel-demo/base/ │ │
│ │ - deployment.yaml (nginx) │ │
│ │ - service.yaml │ │
│ │ - kustomization.yaml │ │
│ └──────────────────────────────────────────────────────┘ │
└─────────────────────┬───────────────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────────┐
│ ArgoCD │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ Detects Git change → Syncs to cluster │ │
│ └──────────────────────────────────────────────────────┘ │
└─────────────────────┬───────────────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────────┐
│ Kind Kubernetes Cluster │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ otel-demo namespace │ │
│ │ - nginx deployment (2 replicas) │ │
│ │ - nginx service │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ litmus namespace │ │
│ │ - Chaos experiments for pod-delete │ │
│ └──────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘

text

## Tech Stack
- **Kubernetes**: Kind (local cluster)
- **GitOps**: ArgoCD
- **Application**: Nginx
- **Chaos Engineering**: Litmus
- **CI/CD**: GitHub Actions

## Data Flow
1. Git push → GitHub Actions validates
2. ArgoCD detects change → Syncs to cluster
3. Kubernetes maintains desired state
4. Litmus injects chaos → Pods restart
5. Kubernetes self-heals → Pods recover
