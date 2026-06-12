# CI/CD Pipeline

## Overview

The Todo Platform uses GitHub Actions to automate code validation, testing, security scanning, container image creation, and GitOps-based deployments.

The pipeline ensures that every change is validated before being released to Kubernetes.

### Technologies

* GitHub Actions
* Vitest
* SonarCloud
* Trivy
* Docker
* GitHub Container Registry (GHCR)
* FluxCD Image Automation

---

## Pipeline Architecture

```text
Developer Push
      |
      v
GitHub Actions
      |
      +--> Lint
      |
      +--> Test + Coverage
      |
      +--> SonarCloud Analysis
      |
      +--> Trivy Security Scan
      |
      +--> Docker Build
      |
      +--> Push Images to GHCR
      |
      v
Flux Image Automation
      |
      v
GitOps Repository Update
      |
      v
Flux Reconciliation
      |
      v
Kubernetes Deployment
```

**Screenshot**

![CI/CD Architecture](screenshots/141-cicd-architecture.png)

---

## Workflow Trigger

The pipeline runs automatically on:

* Push to `main`
* Push to `develop`
* Pull Requests

Workflow Location:

```text
.github/workflows/pipeline.yml
```

**Screenshot**

![GitHub Actions Workflow](screenshots/142-github-actions-workflow.png)

---

## Pipeline Stages

### 1. Linting

Code quality validation using ESLint.

Purpose:

* Enforce coding standards
* Detect syntax issues early

**Screenshot**

![Lint Stage](screenshots/146-lint-stage.png)

---

### 2. Testing & Coverage

Automated tests execute for:

* Auth Service
* Todo Service

Coverage reports are generated during execution.

Current Coverage:

```text
90%+
```

**Screenshot**

![Coverage Report](screenshots/148-coverage-report.png)

---

### 3. SonarCloud Analysis

SonarCloud performs:

* Code Quality Analysis
* Security Analysis
* Coverage Validation

Quality Gate must pass before proceeding.

**Screenshot**

![SonarCloud Dashboard](screenshots/149-sonarcloud-dashboard.png)

---

### 4. Trivy Security Scan

Container images are scanned for vulnerabilities before publishing.

Benefits:

* Early vulnerability detection
* Secure image delivery

**Screenshot**

![Trivy Scan](screenshots/151-trivy-scan.png)

---

### 5. Docker Build & Publish

Docker images are built for:

* frontend
* auth-service
* todo-service

Images are pushed to GitHub Container Registry.

Example:

```text
ghcr.io/<owner>/frontend:<tag>
ghcr.io/<owner>/auth-service:<tag>
ghcr.io/<owner>/todo-service:<tag>
```

**Screenshot**

![GHCR Images](screenshots/155-ghcr-images.png)

---

## GitOps Integration

The CI pipeline does not deploy directly to Kubernetes.

Instead:

```text
GitHub Actions
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

This approach ensures deployments remain fully GitOps-compliant.

**Screenshot**

![GitOps Integration](screenshots/156-gitops-integration.png)

---

## Validation

Verify successful workflow execution:

* GitHub Actions workflow completed successfully
* SonarCloud Quality Gate passed
* Trivy scan completed
* Images available in GHCR
* Flux detected and deployed updated image

**Screenshot**

![Pipeline Success](screenshots/158-pipeline-success.png)

---

## CI/CD Deliverables

| Component          | Status   |
| ------------------ | -------- |
| GitHub Actions     | ✅      |
| Linting            | ✅      |
| Automated Testing  | ✅      |
| Coverage Reporting | ✅      |
| SonarCloud         | ✅      |
| Trivy Scanning     | ✅      |
| Docker Build       | ✅      |
| GHCR Push          | ✅      |
| FluxCD Integration | ✅      |

---

## Summary

The Todo Platform implements an automated CI/CD workflow using GitHub Actions, SonarCloud, Trivy, Docker, GHCR, and FluxCD.

Every code change is validated, tested, scanned, containerized, and delivered through a GitOps workflow, providing a secure and reliable deployment process.
