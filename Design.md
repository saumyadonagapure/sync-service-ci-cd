# CI/CD Design for sync-service

## 1. Branching Strategy

We follow a simplified Git Flow:

- feature/* → development work
- develop → QA environment
- staging → staging environment
- main → production

### Mapping

| Branch   | Environment |
|----------|------------|
| develop  | QA         |
| staging  | Staging    |
| main     | Production |

### Preventing Accidental Production Deployments

- main branch is protected
- PR approval required before merge
- Jenkins manual approval before prod deployment
- Only tagged releases (v*) allowed for production

---

## 2. Jenkins Pipeline

### Stages

1. Checkout code
2. Build (Maven)
3. Unit Testing
4. Static Analysis (SonarQube)
5. Build Docker Image
6. Push Docker Image
7. Deploy to environment
8. Smoke Testing

---

### PR vs Merge Behavior

- PR → build + test only (no deployment)
- Merge to develop → deploy to QA
- Merge to staging → deploy to staging
- Merge to main → deploy to production (manual approval)

---

### Rollback Strategy

- Maintain previous Docker image versions
- Rollback using previous stable image tag
- Manual rollback via script or Jenkins parameter

---

## 3. Configuration Management

### Environment Config

- Separate config per environment (qa, staging, prod)
- Stored in config/*.env files
- Injected at runtime

### Secrets Handling

- MongoDB credentials stored securely
- Use Jenkins credentials / GCP Secret Manager
- Never store secrets in code

---

## 4. Deployment Strategy

### Strategy: Rolling Deployment

- Stop old container
- Start new container with updated image
- Health check using /actuator/health

### Why Rolling?

- Lower cost compared to Blue/Green
- Simpler setup for VM-based deployment

---

### Zero Downtime Approach

- Use multiple instances (if available)
- Update instances one by one
- Route traffic only after health check passes
