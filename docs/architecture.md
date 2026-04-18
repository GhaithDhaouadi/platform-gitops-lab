# Architecture

## Components
- **Kind**: Local Kubernetes cluster
- **ArgoCD**: GitOps continuous delivery
- **OpenTelemetry Demo**: Microservices demo app
- **Prometheus**: Metrics collection
- **Grafana**: Visualization
- **Jaeger**: Distributed tracing
- **Litmus**: Chaos engineering

## Flow
1. Git push → ArgoCD sync
2. OTel Demo runs on Kind
3. Prometheus scrapes metrics
4. Grafana visualizes data
5. Jaeger collects traces
6. Litmus injects chaos
