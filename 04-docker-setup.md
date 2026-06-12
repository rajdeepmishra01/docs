# Docker Setup

## Overview

The Todo Platform uses Docker to containerize all application components, ensuring consistency across development, CI/CD, and Kubernetes environments.

### Containerized Services

- Frontend (React + Vite)
- Auth Service (Node.js)
- Todo Service (Node.js)
- PostgreSQL

---

## Docker Architecture

```text
                Docker Network
                       |
    -----------------------------------------
    |              |              |         |
    v              v              v         v

 Frontend    Auth Service   Todo Service  PostgreSQL
```

**Screenshot**

```text
docs/screenshots/docker-architecture.png
```

---

## Project Structure

Each application component contains its own Dockerfile.

```text
apps/
├── frontend/
├── auth-service/
├── todo-service/
└── database/
```

Docker Compose is used to orchestrate all containers during local development.

---

## Building & Running Containers

Build and start all services:

```bash
docker compose up --build
```

Verify running containers:

```bash
docker ps
```

Expected Containers:

```text
todo-frontend
todo-auth-service
todo-todo-service
todo-postgres
```

**Screenshot**

```text
docs/screenshots/docker-containers.png
```

---

## Container Networking

Containers communicate through a shared Docker network.

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

This enables service discovery without exposing internal services externally.

---

## Persistent Storage

PostgreSQL uses Docker volumes to persist data across container restarts.

Verify:

```bash
docker volume ls
```

**Screenshot**

```text
docs/screenshots/docker-volumes.png
```

---

## Image Verification

Verify locally built images:

```bash
docker images
```

Expected Images:

```text
frontend
auth-service
todo-service
postgres
```

**Screenshot**

```text
docs/screenshots/docker-images.png
```

---

## Container Validation

Verify service health:

```bash
curl http://localhost:5001/health

curl http://localhost:5002/health
```

Expected Response:

```json
{
  "status": "UP"
}
```

**Screenshot**

```text
docs/screenshots/container-health.png
```

---

## Docker & GitOps Workflow

The Docker images created during development are published to GitHub Container Registry (GHCR) and later deployed through FluxCD.

```text
Application Code
       |
       v
Docker Image
       |
       v
GHCR
       |
       v
FluxCD
       |
       v
Kubernetes Cluster
```

**Screenshot**

```text
docs/screenshots/ghcr-images.png
```

---

## Validation

Verify Docker resources:

```bash
docker ps

docker images

docker volume ls
```

---

## Deliverables

| Component | Status |
|------------|---------|
| Dockerfiles | ✅ |
| Docker Compose | ✅ |
| Frontend Container | ✅ |
| Auth Service Container | ✅ |
| Todo Service Container | ✅ |
| PostgreSQL Container | ✅ |
| Networking | ✅ |
| Volumes | ✅ |
| Image Build | ✅ |

---

## Summary

Docker provides the foundation for application portability and deployment consistency across the Todo Platform. All application components are containerized and orchestrated through Docker Compose, forming the basis for CI/CD automation and Kubernetes-based GitOps deployments.