# CI/CD Pipeline

## Overview

The Todo Platform uses GitHub Actions to automate code validation, testing, security scanning, container image creation, and GitOps-based deployments.

### CI/CD Stack

- GitHub Actions
- Vitest
- SonarCloud
- Trivy
- Docker
- GitHub Container Registry (GHCR)
- FluxCD Image Automation

---

## Pipeline Flow

```text
Developer Push
      |
      v
GitHub Actions
      |
      +--> Lint
      +--> Test & Coverage
      +--> SonarCloud Analysis
      +--> Trivy Scan
      +--> Docker Build
      +--> Push to GHCR
      |
      v
Flux Image Automation
      |
      v
GitOps Repository Update
      |
      v
FluxCD Deployment
```

---

## GitHub Actions Workflow

The CI/CD workflow is defined in:

```text
.github/workflows/pipeline.yml
```

Pipeline triggers:

- Push to `main`
- Push to `develop`
- Pull Requests

**Screenshot**

![GitHub Actions Workflow](screenshots/github-actions-workflow.png)

---

## Testing & Code Quality

The pipeline runs automated tests for:

- Auth Service
- Todo Service

Coverage reports are generated during execution.

Current Coverage:

```text
90%+
```

**Screenshot**

![Coverage Report](screenshots/coverage-report.png)

---

## SonarCloud Analysis

SonarCloud validates:

- Code Quality
- Maintainability
- Security Issues
- Coverage Thresholds

The Quality Gate must pass before the pipeline proceeds.

**Screenshot**

![SonarCloud Dashboard](screenshots/sonarcloud-dashboard.png)

---

## Security Scanning

Container images are scanned using Trivy before publication.

Benefits:

- Vulnerability Detection
- Dependency Analysis
- Secure Image Delivery

**Screenshot**

![Trivy Scan](screenshots/trivy-scan.png)

---

## Container Publishing

Application images are built and pushed to GitHub Container Registry (GHCR).

Published Images:

- frontend
- auth-service
- todo-service

Example:

```text
ghcr.io/<owner>/frontend:<tag>
ghcr.io/<owner>/auth-service:<tag>
ghcr.io/<owner>/todo-service:<tag>
```

**Screenshot**

![GHCR Images](screenshots/ghcr-images.png)

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

This approach ensures a fully GitOps-compliant deployment workflow.

---

## Validation

| Component | Status |
|------------|---------|
| GitHub Actions | ✅ |
| Automated Testing | ✅ |
| Coverage Reports | ✅ |
| SonarCloud | ✅ |
| Trivy Scan | ✅ |
| Docker Build | ✅ |
| GHCR Push | ✅ |
| FluxCD Integration | ✅ |

---

## Summary

The Todo Platform implements a complete CI/CD workflow using GitHub Actions, SonarCloud, Trivy, Docker, GHCR, and FluxCD.

Every code change is automatically validated, tested, scanned, containerized, and delivered through a GitOps deployment process.