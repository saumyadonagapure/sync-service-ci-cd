CI/CD and Infrastructure Design for sync-service.

This document describes the CI/CD pipeline and infrastructure design for the sync-service application.  
It ensures reliable deployments, environment isolation, and minimal downtime.

1. Branching Strategy

- main → Production environment
- staging → Pre-production testing
- feature/* → Feature development

Flow:

- Developers create feature branches
- Raise PR to staging
- After testing → merge to main for production

Preventing Accidental Production Deployments

- Main branch is protected
- PR approval required
- No direct commits allowed

---

2. Jenkins Pipeline

Stages:

1. Checkout
2. Build
3. Test
4. Deploy
5. Rollback (conditional)

Artifact:
- Application is built into a JAR/Docker image
- Tagged using BUILD_NUMBER
- Same artifact promoted across environments

Deployment Execution:
- Jenkins updates Cloud Run service with new image
- Health checks validate deployment
- On failure, rollback is triggered automatically
  
PR vs Merge Behavior

- On Pull Request:
  
  - Only Build and Test stages run
  - Ensures code quality

- On Merge to staging:
  
  - Deploy to staging environment

- On Merge to main:
  
  - Deploy to production

Rollback Strategy

- Triggered using parameter "ROLLBACK_TAG"
- Deploys previous stable version

---

3. Configuration Management

- Environment-specific configs:
  
  - qa
  - staging
  - prod

- Managed via:
  
  - Config files
  - Environment variables

Secrets Handling

- MongoDB URI stored in Jenkins Credentials
- Injected as environment variables during deployment
- No secrets stored in code

---

4. Deployment Strategy

Selected: No downtime

Rolling deployment ensures zero downtime by updating instances gradually 
while keeping previous instances serving traffic until new ones are healthy.

Zero/Minimal Downtime Strategy

- Instances updated one by one
- Service remains available during deployment

---

5. Infrastructure Design (GCP)

Compute: Cloud Run

- Fully managed
- Auto scaling
- No server maintenance

Database: MongoDB Atlas

- Managed service
- High availability
- Easy integration

Networking

- Secure HTTPS access
- Controlled ingress

Security & IAM

- Service accounts used
- Secrets stored in Secret Manager
- IAM-based access control

Logging & Monitoring

- GCP Cloud Logging
- GCP Cloud Monitoring
- Alerts for failures

---

6. Architecture Diagram

Developer → GitHub → Jenkins CI/CD Pipeline → Container Build (Docker)
        → Google Cloud Run (Spring Boot Service)
        → MongoDB Atlas (Database)

Supporting Components:
Cloud Run - GCP Secret Manager (secrets)
Cloud Run - Cloud Logging & Monitoring (logs, metrics, alerts)

---

Summary

This system provides:

- Automated CI/CD pipeline
- Environment-based deployments
- Secure configuration and secrets management
- Scalable and cost-effective infrastructure
- Safe rollback mechanism
