# Automated Jenkins CI/CD Pipeline

A containerized React web application paired with a declarative Jenkins CI/CD pipeline (`Jenkinsfile`) and modular Bash automation scripts (`scripts/build.sh`, `scripts/test.sh`) to automate testing, building, and Docker packaging.

## Project Structure
- `Jenkinsfile` — Declarative CI/CD pipeline definition orchestrating build, test, and container stages
- `scripts/build.sh` — Shell script automating application build steps
- `scripts/test.sh` — Shell script executing the automated test suite (`src/App.test.js`)
- `Dockerfile` — Container image definition for packaging the web application
- `src/` & `public/` — React frontend application source and static assets

## Pipeline Stages
1. **Checkout & Dependency Setup:** Pulls source changes and installs Node.js dependencies (`package.json`).
2. **Automated Testing (`scripts/test.sh`):** Runs unit tests to catch regressions before building.
3. **Application Build (`scripts/build.sh`):** Compiles and bundles production frontend assets.
4. **Container Packaging (`Dockerfile`):** Builds a deployable Docker image from the verified build artifacts.

## Local Verification
Run the automation scripts and Docker build locally before pushing pipeline changes:

```bash
# Make scripts executable
chmod +x scripts/build.sh scripts/test.sh

# Run automated tests and build
./scripts/test.sh
./scripts/build.sh

# Build Docker image locally
docker build -t cicd-pipeline-app:local .
```

## Operational & Troubleshooting Notes
- **Script Permissions in CI:** Ensure `scripts/build.sh` and `scripts/test.sh` retain executable permissions (`chmod +x`) in Git so Jenkins agents do not fail with `Permission denied`.
- **Non-Interactive Test Runs:** Set `CI=true` when executing React test runners in Jenkins so Jest exits cleanly after a single pass rather than hanging in watch mode.
