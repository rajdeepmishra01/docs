# Local Development

## Overview

The Todo Platform can be executed locally using Docker Compose before deployment to Kubernetes.

The local environment consists of:

- Frontend (React)
- Auth Service
- Todo Service
- PostgreSQL

This setup allows developers to validate application functionality and API integration before container images are promoted through the CI/CD pipeline.

---

## Prerequisites

Install the following tools:

| Tool | Required |
|--------|----------|
| Docker | ✅ |
| Docker Compose | ✅ |
| Git | ✅ |

Verify:

```bash
docker --version
docker compose version
git --version
```

---

## Project Structure

```text
major-assignment-app/
│
├── apps/
│   ├── frontend/
│   ├── auth-service/
│   ├── todo-service/
│   └── database/
│
└── docker-compose.yml
```

---

## Environment Configuration

Create environment files for backend services.

### Auth Service

```text
apps/auth-service/.env
```

### Todo Service

```text
apps/todo-service/.env
```

Example:

```env
DATABASE_URL=postgres://postgres:postgres@postgres:5432/todoapp
JWT_SECRET=your-secret-key
```

---

## Running the Application

Start all services:

```bash
docker compose up --build
```

Verify running containers:

```bash
docker ps
```

Expected Services:

- frontend
- auth-service
- todo-service
- postgres

**Screenshot**

```text
docs/screenshots/docker-compose-up.png
```

---

## Application Validation

Frontend:

```text
http://localhost:5173
```

Verify:

- User Registration
- User Login
- Todo CRUD Operations

**Screenshot**

```text
docs/screenshots/local-application.png
```

---

## API Health Checks

Auth Service:

```bash
curl http://localhost:5001/health
```

Todo Service:

```bash
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
docs/screenshots/api-health-checks.png
```

---

## Database Persistence

Verify PostgreSQL persistence:

```bash
docker compose down

docker compose up -d
```

Previously created todo items should remain available after restart.

**Screenshot**

```text
docs/screenshots/database-persistence.png
```

---

## Development Workflow

```text
Code Change
     |
     v
Docker Compose
     |
     v
Local Testing
     |
     v
Git Commit
     |
     v
GitHub Actions
     |
     v
Kubernetes Deployment
```

---

## Validation

Verify local environment:

```bash
docker ps

docker compose ps
```

---

## Deliverables

| Component | Status |
|------------|---------|
| Docker Compose | ✅ |
| Frontend | ✅ |
| Auth Service | ✅ |
| Todo Service | ✅ |
| PostgreSQL | ✅ |
| Health Checks | ✅ |
| Database Persistence | ✅ |

---

## Summary

The local development environment provides a complete setup for developing and validating the Todo Platform before deployment. Docker Compose ensures all services run consistently and mirrors the containerized architecture later deployed to Kubernetes.