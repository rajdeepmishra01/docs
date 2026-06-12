# Monitoring and Observability

## Overview

The Todo Platform implements a complete observability stack for monitoring application performance, infrastructure health, and centralized logging.

### Monitoring Stack

* Prometheus
* Grafana
* ServiceMonitor
* Loki
* Vector
* Node Exporter
* Kube State Metrics

---

## Observability Architecture

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

------------------------

 Application Logs
        |
        v
      Vector
        |
        v
       Loki
        |
        v
 Grafana Explore
```

**Screenshot**

![Monitoring Architecture](screenshots/monitoring-architecture.png)

---

## Monitoring Stack

The monitoring components run inside the dedicated `monitoring` namespace.

Verify:

```bash
kubectl get pods -n monitoring
```

Implemented Components:

| Component          | Purpose            |
| ------------------ | ------------------ |
| Prometheus         | Metrics Collection |
| Grafana            | Visualization      |
| Loki               | Log Storage        |
| Vector             | Log Collection     |
| Node Exporter      | Node Metrics       |
| Kube State Metrics | Kubernetes Metrics |

**Screenshot**

![Monitoring Namespace](screenshots/monitoring-stack.png)

---

## Prometheus

Prometheus collects metrics from:

* Kubernetes Cluster
* Application Services
* Infrastructure Components

Service discovery is handled through ServiceMonitor resources.

Verify:

```bash
kubectl get servicemonitors -A
```

**Screenshot**

![Prometheus Targets](screenshots/prometheus-targets.png)

---

## Grafana Dashboards

Grafana provides dashboards for:

* Cluster Health
* CPU Usage
* Memory Usage
* Pod Status
* Application Metrics

**Screenshot**

![Grafana Dashboard](screenshots/grafana-dashboard.png)

---

## Centralized Logging

Logs are collected using Vector and stored in Loki.

Log Flow:

```text
Application Pods
      |
      v
    Vector
      |
      v
     Loki
      |
      v
 Grafana Explore
```

Benefits:

* Centralized Log Management
* Troubleshooting
* Operational Visibility

**Screenshot**

![Loki Logs](screenshots/loki-logs.png)

---

## Validation

Verify monitoring resources:

```bash
kubectl get pods -n monitoring
kubectl get servicemonitors -A
kubectl get daemonsets -A
```

---

## Deliverables

| Component          | Status |
| ------------------ | ------ |
| Prometheus         | ✅      |
| Grafana            | ✅      |
| ServiceMonitor     | ✅      |
| Node Exporter      | ✅      |
| Kube State Metrics | ✅      |
| Loki               | ✅      |
| Vector             | ✅      |
| Dashboards         | ✅      |
| Log Aggregation    | ✅      |

---

## Summary

The Todo Platform implements a complete monitoring and logging solution using Prometheus, Grafana, Loki, and Vector.

The observability stack provides visibility into application performance, infrastructure health, resource utilization, and centralized logs, enabling efficient monitoring and troubleshooting of Kubernetes workloads.
