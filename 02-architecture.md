# Architecture

## Overview

The Todo Platform is a cloud-native microservices application deployed on Kubernetes and managed through GitOps using FluxCD.

The platform consists of:

* React Frontend
* Auth Service
* Todo Service
* PostgreSQL
* Kubernetes
* Helm
* FluxCD
* Prometheus & Grafana
* Loki Logging
* GitHub Actions CI/CD

---

## High-Level Architecture

```text
                           Developer
                               |
                               v
                    GitHub (Application Repo)
                               |
                               v
                      GitHub Actions CI/CD
             (Lint → Test → Sonar → Trivy → Build)
                               |
                               v
                    GitHub Container Registry
                               |
                               v
                     Flux Image Automation
                               |
                               v
                    GitHub (GitOps Repository)
                               |
                               v
                             FluxCD
                               |
                               v

+---------------------------------------------------------+
|                Kubernetes Cluster (k3d)                 |
|                                                         |
|  Ingress                                                |
|     |                                                   |
|     v                                                   |
|  Frontend (React)                                       |
|     |                                                   |
|     +---------> Auth Service ----------+                |
|     |                                  |                |
|     +---------> Todo Service ----------+-----> PostgreSQL
|                                                (PVC)    |
|                                                         |
|  Monitoring Stack                                       |
|  Prometheus --> Grafana                                 |
|  Vector -----> Loki -----> Grafana                      |
|                                                         |
|  Security & Operations                                  |
|  RBAC | NetworkPolicy | TLS | HPA | VPA | CronJobs      |
+---------------------------------------------------------+
```

**Screenshot**

```text
docs/screenshots/platform-architecture.png
```

---

## Application Architecture

The application follows a microservices design.

| Service      | Technology            | Responsibility   |
| ------------ | --------------------- | ---------------- |
| Frontend     | React, Vite           | User Interface   |
| Auth Service | Node.js, Express, JWT | Authentication   |
| Todo Service | Node.js, Express      | Todo Management  |
| PostgreSQL   | PostgreSQL            | Data Persistence |

### Request Flow

```text
User
  |
  v
Ingress
  |
  v
Frontend
  |
  +----> Auth Service ----> PostgreSQL
  |
  +----> Todo Service ----> PostgreSQL
```

**Screenshot**

```text
docs/screenshots/application-flow.png
```

---

## Kubernetes Architecture

The platform runs on a multi-node k3d cluster.

```text
k3d Cluster
│
├── Control Plane
│
├── Worker Node 1
│
└── Worker Node 2
```

Namespaces:

* todo-platform
* todo-platform-dev
* todo-platform-staging
* monitoring
* flux-system

Implemented Resources:

* Deployment
* Service
* Ingress
* ConfigMap
* Secret
* PVC
* HPA
* NetworkPolicy

**Screenshot**

```text
docs/screenshots/kubernetes-architecture.png
```

---

## GitOps Deployment Flow

All deployments are managed through FluxCD.

```text
Code Change
     |
     v
GitHub Actions
     |
     v
Docker Image
     |
     v
GHCR
     |
     v
Flux Image Automation
     |
     v
GitOps Repository
     |
     v
FluxCD
     |
     v
Kubernetes Cluster
```

**Screenshot**

```text
docs/screenshots/gitops-flow.png
```

---

## Observability Architecture

Monitoring and logging are implemented using Prometheus, Grafana, Loki, and Vector.

```text
Application Metrics
        |
        v
 ServiceMonitor
        |
        v
   Prometheus
        |
        v
     Grafana

Application Logs
        |
        v
      Vector
        |
        v
       Loki
        |
        v
     Grafana
```

**Screenshot**

```text
docs/screenshots/observability-architecture.png
```

---

## Security Architecture

Security is implemented through multiple Kubernetes-native controls.

Implemented Controls:

* RBAC
* Network Policies
* Kubernetes Secrets
* TLS Certificates
* ResourceQuota
* LimitRange
* PodDisruptionBudget

```text
User
  |
 HTTPS
  |
 Ingress
  |
 Network Policies
  |
 Application Services
  |
 Kubernetes Secrets
  |
 PostgreSQL
```

**Screenshot**

```text
docs/screenshots/security-architecture.png
```

---

## Summary

The Todo Platform combines microservices, Kubernetes, GitOps, observability, security, autoscaling, and CI/CD into a single cloud-native deployment platform.

The architecture demonstrates a complete end-to-end DevOps workflow from code commit to automated Kubernetes deployment and operational monitoring.
