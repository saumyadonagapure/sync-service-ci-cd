CI/CD Design for sync-service

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

Selected: Rolling Deployment

- Gradual update of instances
- No downtime
- Cost-effective

Zero/Minimal Downtime Strategy

- Instances updated one by one
- Service remains available during deployment

Alternative:

- Blue/Green deployment (not used due to higher cost)

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

6. Architecture Flow

User → Cloud Run → Spring Boot Service → MongoDB Atlas

---

Summary

This system provides:

- Automated CI/CD pipeline
- Environment-based deployments
- Secure configuration and secrets management
- Scalable and cost-effective infrastructure
- Safe rollback mechanism
