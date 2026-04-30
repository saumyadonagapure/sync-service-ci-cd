CI/CD Design for sync-service

1. Branching Strategy

- main → Production environment
- staging → Pre-production testing
- feature/* → Feature development branches

Flow:

- Developers create feature branches
- Raise PR to staging
- After testing → merge to main for production

Safety:

- No direct commits to main
- PR approval required for production

---

2. Jenkins Pipeline

Stages:

1. Checkout
2. Build
3. Test
4. Deploy
5. Rollback (conditional)

Flow:

- On PR → Run build & test
- On merge → Trigger deployment

Rollback:

- Triggered using parameter "ROLLBACK_TAG"
- Deploys previous stable version

---

3. Configuration Management

- Environment-specific configs:
  
  - "qa"
  - "staging"
  - "prod"

- Managed via:
  
  - Config files
  - Environment variables

Secrets:

- Stored in Jenkins Credentials
- Example:
  - MongoDB URI
  - API keys

---

4. Deployment Strategy

Chosen: Rolling Deployment

- Gradual update of instances
- No downtime
- Simple and cost-effective

Alternative:

- Blue/Green (not used due to higher cost)

---

5. Rollback Strategy

- Each build tagged using build number
- Previous version stored
- Rollback triggered manually using parameter

---

Summary

This pipeline ensures:

- Automated CI/CD
- Environment-based deployments
- Safe rollback mechanism
- Scalable and production-ready workflow

