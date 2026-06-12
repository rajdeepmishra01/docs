# Troubleshooting Guide

## Overview

This document summarizes the major issues encountered during the implementation of the Todo Platform and the solutions used to resolve them.

---

## Docker Issues

### PostgreSQL Container Name Conflict

**Issue**

Docker Compose failed to start PostgreSQL.

```text
Conflict. The container name "/todo-postgres"
is already in use.
```

**Resolution**

Remove the existing container and restart the stack.

```bash
docker rm -f todo-postgres
docker compose up --build
```

**Screenshot**

![Docker Container Conflict](screenshots/troubleshoot-docker-conflict.png)

---

### PostgreSQL Image Pull Failure

**Issue**

Docker failed to pull the PostgreSQL image.

```text
TLS handshake timeout
```

**Resolution**

Pull the image manually and retry deployment.

```bash
docker pull postgres:15
```

**Screenshot**

![PostgreSQL Pull Failure](screenshots/troubleshoot-postgres-pull.png)

---

## Kubernetes Issues

### k3d Cluster Creation Failure

**Issue**

k3d cluster creation failed while downloading the proxy image.

```text
ghcr.io/k3d-io/k3d-proxy
TLS handshake timeout
```

**Resolution**

```bash
docker logout ghcr.io
docker pull ghcr.io/k3d-io/k3d-proxy:5.8.3
k3d cluster create todo-platform
```

**Screenshot**

![k3d Proxy Issue](screenshots/troubleshoot-k3d.png)

---

### Persistent Volume Claim Pending

**Issue**

PostgreSQL pod could not start because the PVC remained in a Pending state.

**Resolution**

Verify PVC and StorageClass configuration.

```bash
kubectl get pvc
kubectl get storageclass
```

**Screenshot**

![PVC Issue](screenshots/troubleshoot-pvc.png)

---

## GitOps Issues

### Flux Reconciliation Delay

**Issue**

Changes pushed to GitHub were not immediately reflected in the cluster.

**Resolution**

Force reconciliation.

```bash
flux reconcile source git flux-system
flux reconcile kustomization flux-system
```

Verify:

```bash
flux get all
```

**Screenshot**

![Flux Reconciliation](screenshots/troubleshoot-flux.png)

---

### Image Automation Not Updating Tags

**Issue**

Flux Image Automation did not update deployment manifests.

**Resolution**

Verify ImageRepository, ImagePolicy, and ImageUpdateAutomation resources.

```bash
kubectl get imagerepositories -A
kubectl get imagepolicies -A
kubectl get imageupdateautomations -A
```

**Screenshot**

![Image Automation Issue](screenshots/troubleshoot-image-automation.png)

---

## Monitoring Issues

### Prometheus Targets Not Detected

**Issue**

Application metrics were not visible in Prometheus.

**Root Cause**

ServiceMonitor configuration mismatch.

**Resolution**

```bash
kubectl get servicemonitors -A
```

Verify all targets are marked **UP** in Prometheus.

**Screenshot**

![Prometheus Targets](screenshots/troubleshoot-prometheus.png)

---

### Grafana Dashboard Showing No Data

**Issue**

Grafana dashboards displayed empty panels.

**Resolution**

* Verify Prometheus targets.
* Validate Grafana datasource configuration.
* Refresh dashboards after metrics collection.

**Screenshot**

![Grafana Dashboard Issue](screenshots/troubleshoot-grafana.png)

---

## Autoscaling Issues

### HPA Metrics Unavailable

**Issue**

HPA displayed:

```text
unknown
```

**Resolution**

Verify Metrics Server.

```bash
kubectl top pods -A
```

Check Metrics Server pods:

```bash
kubectl get pods -n kube-system
```

**Screenshot**

![HPA Issue](screenshots/troubleshoot-hpa.png)

---

### VPA Recommendations Missing

**Issue**

VPA showed no recommendations.

**Resolution**

Generate application traffic and allow metrics collection to run.

```bash
kubectl describe vpa
```

**Screenshot**

![VPA Issue](screenshots/troubleshoot-vpa.png)

---

## CI/CD Issues

### SonarCloud Coverage Failure

**Issue**

Coverage was not being detected correctly by SonarCloud.

**Resolution**

Validate coverage report generation.

```bash
cat coverage/lcov.info
```

Ensure application files appear in the report.

**Screenshot**

![SonarCloud Issue](screenshots/troubleshoot-sonar.png)

---

## Useful Debugging Commands

### Cluster

```bash
kubectl get nodes
kubectl get pods -A
kubectl get events -A
```

### FluxCD

```bash
flux get all
```

### Monitoring

```bash
kubectl get pods -n monitoring
```

### Autoscaling

```bash
kubectl get hpa -A
kubectl get vpa -A
```

---

## Key Lessons Learned

* GitOps simplifies Kubernetes operations.
* Monitoring should be configured early.
* Resource requests and limits are essential for autoscaling.
* Image automation reduces deployment effort.
* Observability significantly improves troubleshooting.
* Infrastructure should always be managed as code.

---

## Summary

During implementation, several issues were encountered across Docker, Kubernetes, FluxCD, Monitoring, Autoscaling, and CI/CD. Resolving these challenges improved platform stability and provided practical experience with operating cloud-native applications using GitOps and Kubernetes.
