# Security

## Overview

The Todo Platform implements multiple Kubernetes security controls to protect workloads, network communication, sensitive data, and cluster resources.

### Implemented Controls

* Kubernetes Secrets
* RBAC
* Network Policies
* TLS with cert-manager
* ResourceQuota
* LimitRange
* PriorityClass
* PodDisruptionBudget (PDB)
* Trivy Container Image Scanning

---

## Security Architecture

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

![Security Architecture](screenshots/security-architecture.png)

---

## Access Control (RBAC)

Role-Based Access Control (RBAC) is used to restrict access to Kubernetes resources using:

* Role
* RoleBinding
* ServiceAccount

Verify:

```bash
kubectl get roles,rolebindings -A
```

**Screenshot**

![RBAC](screenshots/rbac.png)

---

## Network Policies

Network Policies restrict communication between workloads and allow only required traffic.

Example:

```text
Frontend
   |
   +--> Auth Service
   |
   +--> Todo Service
            |
            v
        PostgreSQL
```

Verify:

```bash
kubectl get networkpolicy -A
```

**Screenshot**

![Network Policies](screenshots/network-policy.png)

---

## Secrets Management

Sensitive configuration is stored using Kubernetes Secrets.

Examples:

* Database Credentials
* JWT Secret

Verify:

```bash
kubectl get secrets -n todo-platform
```

**Screenshot**

![Secrets](screenshots/secrets.png)

---

## TLS & cert-manager

TLS certificates are automatically managed using cert-manager.

Resources:

* ClusterIssuer
* Certificate
* TLS Secret

Verify:

```bash
kubectl get clusterissuers
kubectl get certificates -A
```

**Screenshot**

![Certificates](screenshots/certificates.png)

---

## Resource Governance

To prevent resource abuse, the platform implements:

* ResourceQuota
* LimitRange
* PriorityClass
* PodDisruptionBudget

Verify:

```bash
kubectl get resourcequota -A
kubectl get limitrange -A
kubectl get priorityclass
kubectl get pdb -A
```

**Screenshot**

![Resource Governance](screenshots/resource-governance.png)

---

## Container Security

Container images are scanned during CI/CD using Trivy.

Benefits:

* Vulnerability Detection
* Dependency Analysis
* Secure Image Delivery

**Screenshot**

![Trivy Scan](screenshots/trivy-scan.png)

---

## Validation

| Component           | Status |
| ------------------- | ------ |
| Secrets             | ✅      |
| RBAC                | ✅      |
| Network Policies    | ✅      |
| TLS                 | ✅      |
| cert-manager        | ✅      |
| ResourceQuota       | ✅      |
| LimitRange          | ✅      |
| PriorityClass       | ✅      |
| PodDisruptionBudget | ✅      |
| Trivy Scanning      | ✅      |

---

## Summary

The Todo Platform follows a defense-in-depth security model using Kubernetes-native controls for access management, workload isolation, secret protection, TLS encryption, resource governance, and container image security.
