# CI/CD Architecture

## Document Information

| Attribute       | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Document Name   | CI/CD Architecture                                        |
| Platform        | NEXO-RAIDEN                                               |
| Environment     | Raspberry Pi 5 Homelab                                    |
| CI Platform     | GitHub Actions                                            |
| CD Platform     | ArgoCD                                                    |
| Repository Type | GitOps                                                    |
| Audience        | Platform Engineers, DevOps Engineers, Cloud Engineers     |
| Purpose         | Define the software delivery architecture for NEXO-RAIDEN |

---

# Executive Summary

The NEXO-RAIDEN CI/CD architecture provides a secure, automated, and repeatable software delivery process.

The platform follows modern GitOps principles where:

* GitHub Actions performs Continuous Integration (CI)
* ArgoCD performs Continuous Delivery (CD)
* Kubernetes executes workloads
* Git remains the source of truth

The objective is to eliminate manual deployments and ensure that all changes are validated, tested, scanned, and auditable before reaching the cluster.

---

# Why CI/CD Exists

## Traditional Deployment Problems

Without CI/CD:

```text
Developer
↓
Manual Build
↓
Manual Testing
↓
Manual Deployment
↓
Human Error
↓
Production Issues
```

Challenges:

* Inconsistent deployments
* Human error
* Missing tests
* Security risks
* No audit trail

---

## Modern GitOps Deployment

With CI/CD:

```text
Developer
↓
Git Push
↓
GitHub Actions
↓
Validation
↓
Testing
↓
Security Scan
↓
Artifact Build
↓
GitOps Update
↓
ArgoCD
↓
Kubernetes
```

Benefits:

* Repeatability
* Traceability
* Security
* Faster delivery
* Reduced operational risk

---

# CI/CD Architecture Overview

## High-Level Architecture

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    ├── Validation
    ├── Testing
    ├── Security Scanning
    ├── Build
    └── Release
    │
    ▼
GitOps Repository
    │
    ▼
ArgoCD
    │
    ▼
K3s Kubernetes Cluster
    │
    ▼
Applications
```

---

# Core Components

## GitHub

### Purpose

Source code management and collaboration platform.

### Responsibilities

* Version control
* Pull requests
* Code review
* Branch protection
* Release management

### Failure Impact

If GitHub is unavailable:

* No code changes
* No deployments
* No pipeline execution

---

## GitHub Actions

### Purpose

Continuous Integration engine.

### Responsibilities

* Validation
* Testing
* Security scanning
* Artifact generation
* Release automation

### Failure Impact

If GitHub Actions fails:

* Deployments stop
* Validation stops
* Security checks stop

---

## ArgoCD

### Purpose

Continuous Delivery engine.

### Responsibilities

* Git synchronization
* Drift detection
* Self-healing
* Kubernetes deployment

### Failure Impact

If ArgoCD fails:

* Deployments stop
* Drift detection stops
* Manual intervention required

---

## Kubernetes

### Purpose

Application runtime environment.

### Responsibilities

* Scheduling
* Networking
* Storage
* Scaling
* Service discovery

---

# Repository Strategy

## Current Repositories

### homelab-gitops

Purpose:

Platform infrastructure and GitOps configuration.

Contains:

* ArgoCD Applications
* Infrastructure manifests
* Documentation
* Security configuration

---

## Future Application Repositories

Examples:

```text
account-service
audit-service
transaction-service
ai-orchestrator
```

Responsibilities:

* Application code
* Unit tests
* Integration tests
* Dockerfiles
* GitHub Actions

---

# Workflow Architecture

## gitops-validation.yml

### Purpose

Validate GitOps repository structure.

### Trigger

```yaml
push
pull_request
```

### Responsibilities

* Validate directories
* Validate manifests
* Prevent repository corruption

### Failure Impact

Broken GitOps changes cannot be merged.

---

## docs-validation.yml

### Purpose

Protect architecture documentation.

### Responsibilities

Verify existence of:

* platform-topology.md
* security.md
* networking.md
* gitops.md
* data-layer.md

### Failure Impact

Documentation standards are violated.

---

## trivy.yml

### Purpose

Perform security scanning.

### Responsibilities

* Vulnerability scanning
* Misconfiguration detection
* Secret detection

### Failure Impact

Security issues may reach production.

---

# End-to-End Delivery Flow

## Current Flow

```text
Engineer
↓
Git Push
↓
GitHub Actions
↓
Validation
↓
Security Scan
↓
Git Repository Updated
↓
ArgoCD Detects Change
↓
Application Sync
↓
Deployment
```

---

# Security Model

## Current Controls

### GitHub

* Authenticated users
* Pull requests
* Commit history

### GitHub Actions

* Controlled workflows
* Repository permissions

### ArgoCD

* GitOps-only deployment model

---

## Future Controls

### Trivy

Container scanning.

### Cosign

Image signing.

### Syft

SBOM generation.

### Dependabot

Dependency updates.

---

# Monitoring and Observability

## GitHub Actions

Monitor:

* Workflow duration
* Failure rates
* Build success rate

## ArgoCD

Monitor:

* Sync status
* Health status
* Drift detection

---

# Failure Scenarios

## Scenario 1

GitHub Actions Failure

Impact:

No builds.

Recovery:

Fix workflow and rerun.

---

## Scenario 2

ArgoCD Failure

Impact:

No deployments.

Recovery:

Restore ArgoCD.

---

## Scenario 3

Invalid Manifest

Impact:

Deployment blocked.

Recovery:

Fix validation errors.

---

# Best Practices

## Required

* Pull Requests
* Code Reviews
* Security Scanning
* GitOps Deployments
* Documentation Updates

## Avoid

* Manual kubectl apply
* Hardcoded secrets
* Direct production changes

---

# Future Evolution

## Phase 1

Current:

* Validation
* Documentation checks
* Security scans

---

## Phase 2

Future:

* Java 21 builds
* Unit testing
* Testcontainers

---

## Phase 3

Future:

* Docker image builds
* GitHub Container Registry
* Trivy image scanning

---

## Phase 4

Future:

* Cosign signing
* SBOM generation
* GitOps promotion

---

# AWS Mapping

| Current        | AWS            |
| -------------- | -------------- |
| GitHub Actions | GitHub Actions |
| K3s            | EKS            |
| Local Registry | ECR            |
| ArgoCD         | ArgoCD on EKS  |
| Vault          | HCP Vault      |

---

# Related Documentation

* platform-topology.md
* gitops.md
* security.md
* backup-strategy.md
* cluster-recovery.md

---

# Summary

The CI/CD architecture provides the automated software delivery foundation of NEXO-RAIDEN.

GitHub Actions performs validation, testing, and security scanning.

ArgoCD performs deployment and reconciliation.

Together they provide a secure, auditable, and production-grade delivery platform that supports the future evolution of NEXO-RAIDEN into AWS, multi-cloud, and AI-native environments.

