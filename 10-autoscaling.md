# Autoscaling

## Overview

The Todo Platform implements Kubernetes autoscaling to automatically adapt to changing workloads.

Implemented components:

* Horizontal Pod Autoscaler (HPA)
* Vertical Pod Autoscaler (VPA)
* Metrics Server

These components improve availability, scalability, and resource efficiency.

---

## Autoscaling Architecture

```text
Application Metrics
        |
        v
   Metrics Server
        |
   +----+----+
   |         |
   v         v
  HPA       VPA
   |         |
   +----+----+
        |
        v
 Application Workloads
```

**Screenshot**

![Autoscaling Architecture](screenshots/autoscaling-architecture.png)

---

## Horizontal Pod Autoscaler (HPA)

The platform uses HPA to automatically increase or decrease pod replicas based on CPU utilization.

Configured Resources:

* frontend-hpa
* auth-service-hpa
* todo-service-hpa

Verify:

```bash
kubectl get hpa -n todo-platform
```

Example Configuration:

```yaml
minReplicas: 2
maxReplicas: 10
averageUtilization: 50
```

**Screenshot**

![HPA Resources](screenshots/hpa-list.png)

---

## Vertical Pod Autoscaler (VPA)

VPA analyzes workload resource consumption and provides CPU and memory recommendations.

Configured Resource:

```text
todo-service-vpa
```

Current Mode:

```yaml
updateMode: Off
```

This allows safe evaluation of recommendations without automatically modifying workloads.

Verify:

```bash
kubectl get vpa -A
```

**Screenshot**

![VPA Recommendations](screenshots/vpa-recommendations.png)

---

## Metrics Server

Metrics Server provides the resource metrics required by both HPA and VPA.

Verify:

```bash
kubectl top pods -A
kubectl top nodes
```

**Screenshot**

![Metrics Server](screenshots/metrics-server.png)

---

## Validation

Verify autoscaling resources:

```bash
kubectl get hpa -A
kubectl get vpa -A
kubectl top pods -A
```

---

## Deliverables

| Component                | Status |
| ------------------------ | ------ |
| Metrics Server           | ✅      |
| HPA                      | ✅      |
| frontend-hpa             | ✅      |
| auth-service-hpa         | ✅      |
| todo-service-hpa         | ✅      |
| VPA                      | ✅      |
| Resource Recommendations | ✅      |

---

## Summary

The Todo Platform uses Kubernetes HPA and VPA to improve scalability and resource utilization. HPA dynamically adjusts replica counts based on workload demand, while VPA provides resource recommendations based on observed application behavior.
