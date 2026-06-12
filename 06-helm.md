# Helm

## Overview

The Todo Platform uses Helm to package, configure, and deploy Kubernetes resources.

Instead of managing individual YAML files, Helm templates generate all required resources using centralized configuration stored in `values.yaml`.

### Managed Resources

- Frontend
- Auth Service
- Todo Service
- PostgreSQL
- Services
- ConfigMaps
- Secrets
- Ingress
- PersistentVolumeClaims
- Horizontal Pod Autoscalers

---

## Helm Chart Structure

The application is packaged as a reusable Helm chart.

```text
helm-charts/
└── todo-platform/
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
```

**Screenshot**

![Helm Chart Structure](screenshots/helm-chart-structure.png)

---

## Chart Configuration

### Chart Metadata

Chart information is defined in:

```text
Chart.yaml
```

Example:

```yaml
apiVersion: v2
name: todo-platform
version: 1.0.0
```

### Environment Configuration

Deployment settings are managed through:

```text
values.yaml
```

Examples:

- Replica Counts
- Resource Limits
- Environment Variables
- Autoscaling Configuration

**Screenshot**

![Values Configuration](screenshots/values-yaml.png)

---

## Resources Managed by Helm

The chart generates the following Kubernetes resources:

| Resource | Purpose |
|-----------|----------|
| Deployment | Application Workloads |
| Service | Internal Communication |
| ConfigMap | Configuration |
| Secret | Sensitive Data |
| PVC | Persistent Storage |
| Ingress | External Access |
| HPA | Autoscaling |

Verify:

```bash
kubectl get deployments -n todo-platform
kubectl get svc -n todo-platform
kubectl get ingress -n todo-platform
kubectl get pvc -n todo-platform
```

**Screenshot**

![Helm Resources](screenshots/helm-resources.png)

---

## Helm Deployment

Install the application:

```bash
helm install todo-platform \
./helm-charts/todo-platform \
-n todo-platform \
--create-namespace
```

Verify:

```bash
helm list -A
```

**Screenshot**

![Helm Release](screenshots/helm-release.png)

---

## Upgrade & Rollback

Helm supports controlled application upgrades and rollback.

Upgrade:

```bash
helm upgrade todo-platform \
./helm-charts/todo-platform \
-n todo-platform
```

Rollback:

```bash
helm rollback todo-platform <revision>
```

Verify:

```bash
helm history todo-platform -n todo-platform
```

---

## GitOps Integration

FluxCD uses HelmRelease resources to deploy the Helm chart automatically.

```text
Git Repository
      |
      v
    FluxCD
      |
      v
 HelmRelease
      |
      v
 Helm Chart
      |
      v
Kubernetes Resources
```

This enables automated and version-controlled deployments.

**Screenshot**

![Helm GitOps Integration](screenshots/helm-gitops.png)

---

## Validation

Verify Helm resources:

```bash
helm list -A

helm status todo-platform -n todo-platform

helm history todo-platform -n todo-platform
```

---

## Deliverables

| Component | Status |
|------------|---------|
| Chart.yaml | ✅ |
| values.yaml | ✅ |
| Templates | ✅ |
| Deployments | ✅ |
| Services | ✅ |
| ConfigMaps | ✅ |
| Secrets | ✅ |
| PVC | ✅ |
| Ingress | ✅ |
| HPA | ✅ |
| Install | ✅ |
| Upgrade | ✅ |
| Rollback | ✅ |

---

## Summary

Helm provides a reusable and maintainable deployment mechanism for the Todo Platform. The Helm chart packages all Kubernetes resources into a single deployable unit and serves as the deployment foundation used by FluxCD for GitOps-based application delivery.