# Kubernetes Core

## Overview

The Todo Platform is deployed on a multi-node Kubernetes cluster created using k3d.

Kubernetes provides workload orchestration, service discovery, self-healing, networking, storage management, and resource governance for the platform.

### Implemented Resources

- Namespace
- Deployment
- Service
- ConfigMap
- Secret
- PersistentVolumeClaim (PVC)
- Ingress
- Resource Requests & Limits

---

## Cluster Architecture

```text
k3d Cluster
│
├── Control Plane
│
├── Worker Node 1
│
└── Worker Node 2
```

Verify:

```bash
kubectl get nodes -o wide
```

**Screenshot**

```text
docs/screenshots/cluster-nodes.png
```

---

## Namespace Organization

Namespaces are used to isolate environments and platform components.

Implemented Namespaces:

```text
todo-platform
todo-platform-dev
todo-platform-staging
monitoring
flux-system
```

Verify:

```bash
kubectl get namespaces
```

**Screenshot**

```text
docs/screenshots/namespaces.png
```

---

## Application Workloads

The platform consists of four Kubernetes deployments.

| Deployment | Purpose |
|------------|----------|
| frontend | User Interface |
| auth-service | Authentication |
| todo-service | Todo Management |
| postgres | Database |

Verify:

```bash
kubectl get deployments -n todo-platform

kubectl get pods -n todo-platform
```

Kubernetes automatically recreates failed pods, providing self-healing capabilities.

**Screenshot**

```text
docs/screenshots/deployments-pods.png
```

---

## Services & Networking

ClusterIP services provide internal communication between workloads.

Implemented Services:

- frontend
- auth-service
- todo-service
- postgres

Verify:

```bash
kubectl get svc -n todo-platform
```

External access is provided through Kubernetes Ingress.

Verify:

```bash
kubectl get ingress -n todo-platform
```

**Screenshot**

```text
docs/screenshots/services-ingress.png
```

---

## Configuration Management

Application configuration is managed using:

- ConfigMaps
- Secrets

Examples:

- Environment Variables
- Database Credentials
- JWT Secrets

Verify:

```bash
kubectl get configmaps -n todo-platform

kubectl get secrets -n todo-platform
```

**Screenshot**

```text
docs/screenshots/configmaps-secrets.png
```

---

## Persistent Storage

PostgreSQL uses PersistentVolumeClaims (PVCs) to retain data across pod restarts and upgrades.

Verify:

```bash
kubectl get pvc -n todo-platform
```

**Screenshot**

```text
docs/screenshots/persistent-storage.png
```

---

## Resource Management

Resource requests and limits are configured for all workloads.

Example:

```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi

  limits:
    cpu: 500m
    memory: 512Mi
```

Benefits:

- Predictable Scheduling
- Resource Isolation
- Autoscaling Support

**Screenshot**

```text
docs/screenshots/resource-limits.png
```

---

## Validation

Verify Kubernetes resources:

```bash
kubectl get deployments -n todo-platform

kubectl get pods -n todo-platform

kubectl get svc -n todo-platform

kubectl get ingress -n todo-platform

kubectl get pvc -n todo-platform
```

---

## Deliverables

| Component | Status |
|------------|---------|
| Cluster | ✅ |
| Namespaces | ✅ |
| Deployments | ✅ |
| Pods | ✅ |
| Services | ✅ |
| ConfigMaps | ✅ |
| Secrets | ✅ |
| PVC | ✅ |
| Ingress | ✅ |
| Resource Limits | ✅ |
| Self-Healing | ✅ |

---

## Summary

Kubernetes provides the foundation for the Todo Platform by managing application workloads, networking, configuration, secrets, storage, and resource allocation.

These core Kubernetes capabilities enable the platform to operate as a scalable, resilient, and production-style cloud-native application.