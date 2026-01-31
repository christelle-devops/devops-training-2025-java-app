# DevOps Training 2025 - Java App

Repository for the Java app used in the course to practice CI/CD DevOps strategies for compiled-language projects.

## Course context
- Tools: Jenkins (on-prem), GitHub/GitLab (SaaS), and Azure DevOps (cloud 360 platform).
- This `feat/base` branch contains the minimal code to start the training and the statements for all practices.
- Each practice has a dedicated statement `.md` file in `feat/base`. The solution lives in the `training-x-title` branch for that practice.
- This README will be updated incrementally as the course progresses.
- All documentation and source code are in English.

## Project repositories and branches
- Python app: https://github.com/contreras-adr/devops-training-2025-python-app/tree/feat/base
- Java app: https://github.com/contreras-adr/devops-training-2025-java-app/tree/feat/base
- IaC/DevOps: https://github.com/contreras-adr/devops-training-2025-iac-devops/tree/feat/base
- Practice 1 solution branches:
  - https://github.com/contreras-adr/devops-training-2025-python-app/tree/training-1-jenkins-config
  - https://github.com/contreras-adr/devops-training-2025-java-app/tree/training-1-jenkins-config
  - https://github.com/contreras-adr/devops-training-2025-iac-devops/tree/training-1-jenkins-config
- Practice 2 solution branches:
  - https://github.com/contreras-adr/devops-training-2025-python-app/tree/training-2-piplines-ci-cd-jenkins
  - https://github.com/contreras-adr/devops-training-2025-java-app/tree/training-2-piplines-ci-cd-jenkins
  - https://github.com/contreras-adr/devops-training-2025-iac-devops/tree/training-2-piplines-ci-cd-jenkins

## Repository purpose
- Simple Java application with container packaging and execution.
- Base for pipelines, tests, and quality analysis in CI/CD.

## Practical cases (5)
There will be five practical cases, each with a single solution branch named `training-x-title`.
- training-1-jenkins-config (ONGOING)
- training-2-piplines-ci-cd-jenkins (PENDING)
- training-3-github-actions (PENDING)
- training-4-gitlab-ci-cd (PENDING)
- training-5-azure-devops-pipelines (PENDING)

## Local usage guide

### Build the Java app Docker image
```bash
# docker build -t scalian_training-java-hello-world-build:0.0.2-SNAPSHOT --build-arg VERSION=0.0.2-SNAPSHOT -f devops/build.Dockerfile .
docker build -t scalian_training-java-hello-world:0.0.2 --build-arg VERSION=0.0.2-SNAPSHOT -f devops/Dockerfile .

docker run -d --rm -p 8085:8080 --name java-app scalian_training-java-hello-world:0.0.2
curl localhost:8084/hello
docker stop java-app
```

### Verify SonarQube project
```bash
# Prevent next issue
# https://stackoverflow.com/questions/57998092/docker-compose-error-bootstrap-checks-failed-max-virtual-memory-areas-vm-ma
sudo sysctl -w vm.max_map_count=262144
docker-compose up -d sonarqube
docker-compose logs sonarqube

# Create project in sonarqube by accessing the browser "localhost:9009"
# Project name: "devops-training-2025-java-app"
docker networks ls
docker run \
    --rm -w /app \
    -v "c:/Users/a.contreras/Documents/workspaces/training/scalian-devops-2024/2025/java-project-base:/app" \
    --network java-project-base_java-project-net \
    maven:3.8.6-openjdk-11-slim \
    mvn verify sonar:sonar \
    -Dsonar.projectKey=devops-training-2025-java-app \
    -Dsonar.host.url=http://172.16.234.10:9000 \
    -Dsonar.login=sqp_01291ee195139732bf0509b512c5f8dd4ccc8bf9

docker-compose down
```
