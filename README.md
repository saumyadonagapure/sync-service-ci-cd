# sync-service-ci-cd

This project demonstrates a complete CI/CD pipeline and infrastructure design for a Spring Boot service.

## Includes:
- Jenkins CI/CD pipeline (Build, Test, Deploy, Rollback)
- Branching and PR strategy
- Environment-based deployments (qa, staging, prod)
- Configuration and secrets management
- GCP infrastructure design (Cloud Run + MongoDB Atlas)

## Pipeline Flow:
GitHub → Jenkins → Build → Test → Deploy → Rollback (if needed)

## Files:
- Jenkinsfile → Pipeline definition
- Design.md → CI/CD and infrastructure design
- config/ → Environment configs
- scripts/ → Deployment scripts

## Author
Saumya Donagapure
