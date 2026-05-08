<div align="center">

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=flat&logo=amazon-aws&logoColor=white)
![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=flat&logo=terraform&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=flat&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=flat&logo=githubactions&logoColor=white)
![SonarCloud](https://img.shields.io/badge/SonarCloud-F3702A?style=flat&logo=SonarCloud&logoColor=white)

</div>

## GitOps-Driven Infrastructure & App Deployment on AWS EKS

### Overview
This project implements a full GitOps pipeline with two distinct workflows:

Infrastructure as Code (IaC) — Manages AWS infrastructure using Terraform, triggered on changes to infrastructure code.
Application Build & Deploy — Builds, tests, and deploys the application to Amazon EKS using Maven, Docker, and Helm.

All changes originate from a developer's IDE (VS Code) via a git push and are fully automated through GitHub Actions runners.

This repository also includes:
- `Dockerfile` for containerized deployment
- `Jenkinsfile` for CI/CD pipeline
- `ansible/` automation playbooks
- `kubernetes/` GitOps deployment manifests

---

## Steps
- Fetch Code — GitHub Actions runner checks out the repository.
- Maven Build — Compiles and packages the application.
- Static Code Analysis — SonarCloud scans the codebase for code quality and security issues.
- Docker Build & Push — Builds a Docker image and pushes it to Amazon ECR.
- Helm Deploy — Deploys the new image to Amazon EKS using Helm charts.

## Technology Stack

| Category               | Tool           |
|------------------------|----------------|
| IDE                    | VS Code        |
| Version Control        | Git / GitHub   |
| CI/CD                  | GitHub Actions |
| Infrastructure as Code | Terraform      |
| Build Tool             | Maven          |
| Static Code Analysis   | SonarCloud     |
| Containerization       | Docker         |
| Package Manager (K8s)  | Helm           |
| Container Registry     | Amazon ECR     |
| Kubernetes             | Amazon EKS     |
| Cloud Provider         | AWS            |


## Prerequisites

- AWS account with appropriate IAM permissions
- GitHub repository with Actions enabled
- AWS CLI configured locally
- Terraform >= 1.0
- Docker
- kubectl and helm CLIs
- Java / Maven (for local builds)
- VS Code (recommended IDE)

---

## CI/CD

The `Jenkinsfile` defines a build pipeline with these stages:
- Maven build and artifact archive
- Unit tests
- Integration tests
- Checkstyle analysis
- SonarQube analysis
- Artifact publishing to Nexus

---

## Repository Structure
- `pom.xml` — Maven build configuration
- `Dockerfile` — Docker image build instructions
- `Jenkinsfile` — CI/CD pipeline definition
- `ansible/` — deployment automation playbooks
- `kubernetes/` — Kubernetes manifests

---

# Architecture Diagram
```
Developer IDE (VS Code)
        │
        │ Push Code
        ▼
 ┌──────────────────────────────────────────────────────────┐
 │                  CI/CD & INFRASTRUCTURE                   │
 │                                                           │
 │  ┌─────────────────────────┐  ┌────────────────────────┐ │
 │  │  Infrastructure as Code │  │  Application Build &   │ │
 │  │       (IaC)             │  │       Deploy           │ │
 │  │                         │  │                        │ │
 │  │  GitHub Actions Runner  │  │  GitHub Actions Runner │ │
 │  │    ↓ Fetch Code         │  │    ↓ Fetch Code        │ │
 │  │  Terraform Plan/Test    │  │  Maven Build           │ │
 │  │    ↓                    │  │  (+ SonarCloud)        │ │
 │  │  Pull Request Merge     │  │    ↓ Docker            │ │
 │  │    ↓                    │  │    ↓ Helm Charts       │ │
 │  │  Terraform Apply        │  │                        │ │
 │  │  (Main Branch)          │  │                        │ │
 │  └──────────┬──────────────┘  └──────────┬─────────────┘ │
 │             │ Wait for Cluster            │               │
 └─────────────┼─────────────────────────────┼──────────────┘
               │                             │
               ▼                             ▼
        ┌──────────────────────────────────────────┐
        │           AWS CLOUD DEPLOYMENT            │
        │                                           │
        │  AWS Cloud Infrastructure                 │
        │                                           │
        │  VPC Subnet:                              │
        │    Amazon ECR ──(Pull)──▶ Amazon EKS      │
        └──────────────────────────────────────────┘
```