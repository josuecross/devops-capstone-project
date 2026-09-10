# DevOps Capstone Project — CI/CD, Containers & Kubernetes

![Build Status](https://github.com/josuecross/devops-capstone-project/actions/workflows/ci-build.yaml/badge.svg)

A hands-on DevOps and software-delivery project built as part of the **IBM DevOps and Software Engineering Professional Certificate**. The repository demonstrates a Python/Flask REST microservice backed by PostgreSQL, automated testing and linting, Docker packaging, GitHub Actions CI, Kubernetes deployment manifests, and a Tekton delivery pipeline.

> This project began from the official IBM Skills Network capstone starter code. My work completed and extended the required REST endpoints, tests, CI/CD configuration, container/deployment assets, and pipeline tasks. The repository retains IBM's original Apache 2.0 license and attribution.

## What this project demonstrates

- Python/Flask REST API development
- PostgreSQL-backed application behavior
- Test-driven development and automated API/model tests
- GitHub Actions CI with a containerized build environment
- Service dependency health checks before test execution
- Flake8 linting and Nose test automation
- Docker containerization
- Kubernetes Deployment and Service manifests
- Tekton pipeline tasks for build, test, and deployment workflows
- Git branching and pull-request-based development

## Application architecture

```text
Client
  |
  v
Flask REST API
  |
  +-- routes.py       -> HTTP endpoints and request handling
  +-- models.py       -> Account model and persistence logic
  +-- common/         -> logging, status codes, error handlers
  |
  v
PostgreSQL
```

The service exposes an Account resource with create, list, read, update, and delete operations.

### Example endpoints

```text
GET    /health
POST   /accounts
GET    /accounts
GET    /accounts/{id}
PUT    /accounts/{id}
DELETE /accounts/{id}
```

The application separates request handling from model/persistence behavior and returns explicit HTTP status codes for successful and unsuccessful operations.

## Continuous Integration with GitHub Actions

The GitHub Actions workflow runs for pushes and pull requests targeting `main`.

```text
Checkout
   |
   v
Python 3.9 container
   |
   +--> PostgreSQL service container
   |       |
   |       +--> pg_isready health check
   |
   v
Install dependencies
   |
   v
Flake8 validation
   |
   v
Nose test suite
```

A PostgreSQL service container is declared as part of the CI job. The workflow waits for database readiness before running the application test suite, making the database dependency explicit and repeatable in CI.

The credentials used in the CI definition are disposable local/test values for the isolated workflow service container; no production credentials are stored in this repository.

## Testing

Tests cover the application model and REST routes, including normal and failure behavior.

```bash
nosetests
```

The project follows the capstone's test-driven development approach: define expected behavior in tests, implement the corresponding endpoint/model logic, and rerun the suite to validate the result.

## Containerization

The repository includes a `Dockerfile` for packaging the Flask service into a container image.

```bash
docker build -t account-service .
```

The application can also be run with a local PostgreSQL container using the provided Makefile targets.

## Kubernetes deployment

Deployment assets under `deploy/` define:

- a Kubernetes `Deployment` for the application;
- a Kubernetes `Service` for network access;
- environment-driven database configuration.

The deployment configuration consumes database credentials through Kubernetes references rather than embedding application credentials directly in the workload definition.

## Tekton pipeline

The `tekton/` directory contains pipeline and task definitions used in the capstone's continuous-delivery workflow.

The pipeline demonstrates the progression from source code through validation and build stages toward deployment. I implemented and committed pipeline tasks incrementally as part of the exercise, including test, build, and deploy stages.

## Repository structure

```text
.github/workflows/
  ci-build.yaml        # GitHub Actions CI

service/
  common/              # shared logging/error/status helpers
  config.py            # application/database configuration
  models.py            # Account persistence model
  routes.py            # REST API endpoints

tests/                 # unit/API tests and factories

deploy/
  deployment.yaml      # Kubernetes Deployment
  service.yaml         # Kubernetes Service

tekton/
  pipeline.yaml        # Tekton pipeline
  tasks.yaml           # pipeline tasks
  pvc.yaml             # pipeline workspace storage

Dockerfile             # application container image
Makefile               # local development helpers
```

## Running locally

### Install dependencies

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### Start PostgreSQL

If Docker is available:

```bash
make db
```

### Run tests

```bash
nosetests
```

### Run the application

```bash
flask run
```

## Engineering lessons demonstrated

This project reinforced several practical delivery principles:

- a pipeline passing and an application actually working are separate things to validate;
- external dependencies such as databases need explicit readiness handling in CI;
- automated checks are easier to diagnose when linting, dependency setup, and tests are separate stages;
- containerized environments improve repeatability between development and CI;
- deployment manifests should keep configuration and secrets outside application code;
- version-control history and pull requests provide a traceable path from implementation to validation.

## Project origin and attribution

This repository was completed as part of the **IBM DevOps Capstone Project** in the IBM DevOps and Software Engineering Professional Certificate. IBM supplied starter/template material for the course; the implementation and pipeline work in this repository reflects my completed capstone exercises.

Original course attribution and licensing remain under the repository's Apache 2.0 `LICENSE`.

## Author

**Josue David Cruz Lopez**  
Costa Rica  
GitHub: [@josuecross](https://github.com/josuecross)  
LinkedIn: [josue-david-c](https://www.linkedin.com/in/josue-david-c/)
