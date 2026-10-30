# CI/CD Tools and Practices Final Project: `ci-cd-final-project`

## Project Information
- **Project Name:** ci-cd-final-project
- **Course:** Continuous Integration and Continuous Delivery (CI/CD)
- **Course Provider:** IBM (Coursera DevOps and Software Engineering Professional Certificate)
- **Author:** Pranav (GitHub: [@pranavss73](https://github.com/pranavss73))

## Project Overview
This repository contains the completed final project for the **Continuous Integration and Continuous Delivery (CI/CD)** course. It demonstrates an end-to-end automated pipeline for a RESTful Counter Microservice. 

The pipeline incorporates:
1. **Continuous Integration (CI) with GitHub Actions**:
   - Automated linting with `flake8`.
   - Automated unit testing with code coverage using `nose` (`nosetests`).
2. **Continuous Delivery / Deployment (CD) with Tekton & Red Hat OpenShift Pipelines**:
   - Workspace cleanup with custom `cleanup` Tekton task.
   - Source code checkout using `git-clone` task to a PersistentVolumeClaim (`pipeline-pvc`).
   - Linting using `flake8`.
   - Unit testing using custom `nose` Tekton task.
   - Container image building with `buildah`.
   - Automated deployment and rollout to Red Hat OpenShift using `openshift-client`.

## Tech Stack
- **Language**: Python 3.9 (Flask RESTful API)
- **CI Tool**: GitHub Actions (`.github/workflows/workflow.yml`)
- **CD Tool**: Tekton Pipelines (`.tekton/tasks.yml`)
- **Containerization & Orchestration**: Docker, Kubernetes, Red Hat OpenShift

## Local Development & Testing
Install dependencies and run tests:
```bash
python -m pip install -r requirements.txt
flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
nosetests -v --with-spec --spec-color --with-coverage --cover-package=app
```

## License
Licensed under the Apache License 2.0.
