# DevOps Capstone — Python Microservice & CI/CD

**Flask + PostgreSQL microservice completed as part of the IBM DevOps and Software Engineering Professional Certificate, with REST API work, automated tests, GitHub Actions CI, Docker/Kubernetes deployment assets, and Tekton pipeline exercises.**

This repository started from the official IBM Skills Network capstone template and was completed through the course's test-driven development, CI/CD, container, Kubernetes, and Tekton assignments.

> **Attribution:** the base application and course structure originate from the IBM DevOps Capstone Project. My work in this repository is the implementation and completion of the assigned API, testing, CI/CD, container/deployment, and pipeline tasks. The original IBM copyright and Apache 2.0 license are preserved.

## What this project demonstrates

- Python/Flask REST API development
- PostgreSQL-backed application testing
- Test-driven development and automated regression checks
- GitHub Actions continuous integration
- Containerized CI using a Python image and PostgreSQL service
- Linting with Flake8
- Docker application packaging
- Kubernetes deployment/service manifests
- Tekton pipeline construction
- Git branching and pull-request-based course workflow
- Troubleshooting CI stages as separate application, dependency, database, and environment concerns

## Application

The capstone uses an **Account** microservice. Its API supports CRUD-style account operations such as:

```text
POST   /accounts
GET    /accounts
GET    /accounts/{id}
PUT    /accounts/{id}
DELETE /accounts/{id}
GET    /health
```

The application separates persistent model behavior from HTTP routing:

```text
Client
  |
  v
Flask REST routes
  |
  v
Account model / business logic
  |
  v
PostgreSQL
```

## Repository structure

```text
.
├── service/
│   ├── common/          # logging, status codes, error handlers
│   ├── config.py        # Flask/application configuration
│   ├── models.py        # Account persistence model
│   └── routes.py        # REST endpoints
├── tests/               # model, route, CLI, and factory tests
├── .github/workflows/
│   └── ci-build.yaml    # GitHub Actions CI
├── deploy/
│   ├── deployment.yaml  # Kubernetes Deployment
│   └── service.yaml     # Kubernetes Service
├── tekton/
│   ├── pipeline.yaml
│   ├── tasks.yaml
│   └── pvc.yaml
├── Dockerfile
├── Makefile
└── requirements.txt
```

## GitHub Actions CI

The repository includes a CI workflow triggered by pushes and pull requests to `main`.

The job runs in a Python container and starts PostgreSQL as a service dependency:

```text
checkout
   |
   v
install dependencies
   |
   v
Flake8 validation
   |
   v
Nose test suite
   |
   v
result
```

The PostgreSQL service includes a health check so application tests do not begin before the database is ready.

This is a useful example of an important CI principle: a failed pipeline can originate from the source code, dependency installation, database readiness, or environment configuration, so each stage needs its own observable result.

## Local development

### Install

```bash
git clone https://github.com/josuecross/devops-capstone-project.git
cd devops-capstone-project
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

The original course environment can also be initialized with:

```bash
source bin/setup.sh
```

### Run tests

The application tests expect a PostgreSQL database. The included Makefile/course tooling can start the local database environment:

```bash
make db
nosetests
```

### Run the service

```bash
flask run
```

## Container and Kubernetes work

The repository includes:

- a `Dockerfile` for packaging the application;
- Kubernetes `Deployment` and `Service` manifests;
- local K3D-related course tooling through the Makefile;
- Tekton pipeline/task definitions used in the continuous-delivery portion of the capstone.

These artifacts connect application development with the delivery environment rather than treating CI/CD as an isolated YAML exercise.

## Engineering takeaways

The most valuable part of this project for me was connecting several layers of software delivery:

1. implement application behavior;
2. define automated tests for that behavior;
3. reproduce the required database environment;
4. run validation consistently in CI;
5. package the service;
6. describe deployment through Kubernetes manifests;
7. build pipeline tasks that move changes through the delivery workflow.

It also reinforced the distinction between a pipeline step completing and the deployed application actually behaving correctly.

## Course context and license

This repository is based on the **IBM DevOps Capstone Project** from the IBM DevOps and Software Engineering Professional Certificate.

Course information:
- [IBM DevOps Capstone Project](https://www.coursera.org/learn/devops-capstone-project?specialization=devops-and-software-engineering)
- [IBM DevOps and Software Engineering Professional Certificate](https://www.coursera.org/professional-certificates/devops-and-software-engineering)

The repository retains the original **Apache License 2.0** and IBM attribution.

## Portfolio relevance

This project demonstrates hands-on experience connecting **Python, REST APIs, PostgreSQL, automated testing, GitHub Actions, Docker, Kubernetes, and CI/CD concepts** in one delivery workflow.

## Author / Student

**Josue David Cruz Lopez**  
Costa Rica  
GitHub: [@josuecross](https://github.com/josuecross)  
LinkedIn: [josue-david-c](https://www.linkedin.com/in/josue-david-c/)
