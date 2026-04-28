# Kubernetes Infrastructure as Code (k8s-infra)

A production-ready Kubernetes infrastructure project demonstrating GitOps best practices using **ArgoCD** for declarative infrastructure and application management.

## 🎯 Project Overview

This repository contains the infrastructure configuration for a Kubernetes cluster managed through a GitOps workflow. It showcases modern DevOps practices including Infrastructure as Code (IaC), continuous deployment, and automated reconciliation.

### Key Features

- **GitOps Workflow**: Fully declarative infrastructure defined in Git using ArgoCD
- **Automated Deployments**: Self-healing and auto-pruning applications
- **Infrastructure Bootstrap**: Root application pattern for managing cluster bootstrapping
- **Multi-Environment Support**: Separate configurations for different deployment environments
- **Ingress Management**: Traefik-based ingress controller with DNS routing
- **Monitoring Infrastructure**: Scalable monitoring and observability foundation

## 📁 Directory Structure

```
k8s-infra/
├── bootstrap/           # Bootstrap applications for cluster initialization
│   └── root-app.yaml    # Root ArgoCD application that bootstraps the cluster
├── infra/               # Core infrastructure components
│   ├── root.yaml        # Infrastructure namespace definitions
│   ├── argocd/          # ArgoCD configuration and setup
│   │   ├── config-map.yaml    # ArgoCD server parameters
│   │   └── ingress.yaml       # ArgoCD UI ingress with Traefik
│   └── monitoring/      # Monitoring and observability stack
└── apps/                # Application deployments
    ├── k8s-gitops-app.yaml    # GitOps application deployment
    └── k8s-infra.yaml         # Infrastructure app references
```

## 🚀 Getting Started

### Prerequisites

- Kubernetes cluster (v1.21+)
- `kubectl` CLI configured
- ArgoCD installed on the cluster
- Traefik ingress controller

### Installation

1. **Bootstrap the cluster**:

   ```bash
   kubectl apply -f bootstrap/root-app.yaml
   ```

2. **Deploy infrastructure**:

   ```bash
   kubectl apply -f infra/root.yaml
   ```

3. **Deploy applications**:

   ```bash
   kubectl apply -f apps/k8s-gitops-app.yaml
   ```

4. **Access ArgoCD UI**:
   - Configure your `/etc/hosts` or DNS to point `argo.lan` to your cluster
   - Navigate to `http://argo.lan`

## 🏗️ Architecture Highlights

### Bootstrap Pattern

The `root-app.yaml` uses ArgoCD's application of applications pattern, enabling:

- Single entry point for cluster initialization
- Automatic propagation of changes across all applications
- Simplified cluster recovery and reproduction

### GitOps Workflow

- **Source of Truth**: Git repository contains all desired state
- **Automatic Reconciliation**: ArgoCD continuously syncs cluster state with Git
- **Audit Trail**: Complete history of infrastructure changes
- **Rollback Capability**: Easy reversion to previous configurations

### Ingress Configuration

- **Controller**: Traefik (lightweight, dynamic)
- **Hostname**: `argo.lan` for local development
- **Service Routing**: Dynamic DNS-based routing to services

## 🔧 Configuration Details

### ArgoCD Settings

- **Auto Sync**: Enabled for automated deployments
- **Auto Prune**: Removes resources deleted from Git
- **Self Heal**: Automatically corrects cluster drift
- **Insecure Mode**: Configured for development (adjust for production)

### Application Sync Policy

```yaml
syncPolicy:
  automated:
    prune: true # Remove resources no longer in Git
    selfHeal: true # Fix any manual cluster changes
```

## 📊 Key Technologies

| Component               | Technology | Purpose                    |
| ----------------------- | ---------- | -------------------------- |
| Container Orchestration | Kubernetes | Cluster management         |
| GitOps                  | ArgoCD     | Declarative deployments    |
| Ingress Controller      | Traefik    | API gateway & routing      |
| Configuration           | YAML/Helm  | Infrastructure definitions |

## 🔐 Best Practices Implemented

✅ **Declarative Infrastructure**: All resources defined in Git
✅ **Immutable Deployments**: GitOps ensures consistency
✅ **Automated Rollbacks**: Self-healing corrects drift
✅ **Resource Cleanup**: Auto-pruning prevents orphaned resources
✅ **Single Cluster Entry**: Bootstrap pattern for reproducibility
✅ **Namespace Isolation**: Separation of concerns

## 🛠️ Development & Management

### View Application Status

```bash
kubectl get applications -n argocd
kubectl describe application root-application -n argocd
```

### Manual Sync (if needed)

```bash
argocd app sync root-application
```

### Check Cluster Reconciliation

```bash
kubectl get all -n argocd
```

## 📈 Extensibility

This infrastructure is designed for easy expansion:

- Add new applications by creating additional Helm charts or Kustomize overlays
- Extend monitoring stack in `infra/monitoring/`
- Scale to multi-cluster deployments using ArgoCD's multi-cluster features
- Implement secrets management via sealed-secrets or external-secrets

## 🚢 Production Considerations

For production deployments, consider:

1. **TLS/SSL**: Enable HTTPS on Ingress and ArgoCD
2. **Authentication**: Configure OIDC or OAuth2 for ArgoCD
3. **RBAC**: Implement Role-Based Access Control
4. **Secrets Management**: Use sealed-secrets or HashiCorp Vault
5. **Backup Strategy**: Implement Velero for cluster backups
6. **Monitoring**: Deploy full observability stack (Prometheus, Grafana, Loki)
7. **High Availability**: Configure HA for ArgoCD and critical components

## 📚 Resources

- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Traefik Documentation](https://doc.traefik.io/)
- [GitOps Best Practices](https://www.gitops.tech/)

## 📝 License

This project is part of a portfolio demonstrating DevOps and Kubernetes expertise.

---

**Author**: Paul  
**Last Updated**: April 2026
