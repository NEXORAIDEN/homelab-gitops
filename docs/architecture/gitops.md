# GitOps Architecture

## Document Information

| Attribute     | Value                                                |
| ------------- | ---------------------------------------------------- |
| Document Name | GitOps Architecture                                  |
| Platform      | NEXO-RAIDEN                                          |
| GitOps Engine | ArgoCD                                               |
| Repository    | homelab-gitops                                       |
| Environment   | K3s                                                  |
| Purpose       | Define how platform changes are deployed and managed |

---

# Executive Summary

GitOps is the operational model used by NEXO-RAIDEN.

The Git repository is the single source of truth.

No platform component should be modified manually if the change can be represented in Git.

The objective is:

* Consistency
* Repeatability
* Auditability
* Security
* Self-Healing

---

# Why GitOps Exists

## Traditional Operations

```text
Engineer
↓
kubectl apply
↓
Cluster Changes
↓
Nobody Knows What Changed
```

Problems:

* Configuration drift
* Manual errors
* No audit trail
* Difficult recovery

---

## GitOps Operations

```text
Engineer
↓
Git Commit
↓
Git Push
↓
ArgoCD Detects Change
↓
Cluster Updated
```

Benefits:

* Traceability
* Recovery
* Versioning
* Automation

---

# GitOps Principles

## Git is the Source of Truth

Everything must originate from Git.

Examples:

* Applications
* Namespaces
* Certificates
* Secrets Configuration
* Networking
* Monitoring

---

## Declarative Infrastructure

Infrastructure describes desired state.

Example:

```yaml
replicas: 1
```

The cluster continuously reconciles itself to match Git.

---

## Self-Healing

ArgoCD continuously checks:

```text
Git
vs
Cluster
```

If drift is detected:

```text
ArgoCD
↓
Automatic Correction
```

---

# Architecture Overview

```text
GitHub
    │
    ▼
Git Repository
    │
    ▼
ArgoCD
    │
    ▼
Kubernetes API
    │
    ▼
Cluster Resources
```

---

# Repository Structure

## Root Layout

```text
homelab-gitops/
│
├── apps/
├── infrastructure/
├── clusters/
├── docs/
└── backups/
```

---

## apps/

Purpose:

ArgoCD Application definitions.

Examples:

```text
apps/
├── vault.yaml
├── keycloak.yaml
├── postgresql.yaml
├── cert-manager.yaml
```

Each file defines how ArgoCD deploys a platform component.

---

## infrastructure/

Purpose:

Actual Kubernetes resources.

Examples:

```text
infrastructure/
├── security/
├── platform/
├── data/
```

Contains:

* Deployments
* Services
* Ingresses
* Secrets References
* PVCs

---

## clusters/

Purpose:

Cluster-specific configuration.

Examples:

```text
clusters/lab-01/
```

Contains:

* AppProjects
* Root Applications
* Environment configuration

---

# ArgoCD Architecture

## Current Components

```text
argocd-server
argocd-repo-server
argocd-application-controller
argocd-applicationset-controller
argocd-dex
argocd-redis
```

---

## Responsibilities

### argocd-server

User interface and API.

### repo-server

Reads Git repositories.

### application-controller

Performs reconciliation.

### applicationset-controller

Generates applications dynamically.

---

# Current Platform Applications

Document every application.

## nexo-raiden-vault

Purpose:

Secret Management.

Dependencies:

* Namespace
* Storage

---

## nexo-raiden-external-secrets

Purpose:

Synchronize Vault secrets.

Dependencies:

* Vault

---

## nexo-raiden-cert-manager

Purpose:

Certificate lifecycle.

Dependencies:

* Kubernetes CRDs

---

## nexo-raiden-postgresql

Purpose:

Persistent data storage.

Dependencies:

* PVC
* External Secrets

---

## nexo-raiden-keycloak

Purpose:

Identity Provider.

Dependencies:

* PostgreSQL
* External Secrets
* cert-manager

---

# GitOps Deployment Flow

## Infrastructure Change

```text
Engineer
↓
Git Commit
↓
Git Push
↓
GitHub
↓
ArgoCD Sync
↓
Cluster Update
```

---

## Secret Change

```text
Vault
↓
External Secrets
↓
Kubernetes Secret
↓
Application
```

---

## Certificate Change

```text
Ingress
↓
cert-manager
↓
Certificate
↓
TLS Secret
↓
HTTPS
```

---

# Drift Management

## Definition

Drift occurs when:

```text
Git State
≠
Cluster State
```

---

## Detection

ArgoCD continuously compares:

```text
Desired State
vs
Actual State
```

---

## Examples

### Manual kubectl change

ArgoCD restores Git version.

### Deleted resource

ArgoCD recreates resource.

### Configuration mismatch

ArgoCD re-syncs.

---

# Failure Scenarios

## GitHub Failure

Impact:

No new deployments.

Existing workloads continue.

---

## ArgoCD Failure

Impact:

No synchronization.

Existing workloads continue.

---

## Cluster Failure

Impact:

Platform unavailable.

Recovery:

cluster-recovery.md

---

# Operational Procedures

## Check Application Status

```bash
kubectl get applications -n argocd
```

---

## Refresh Application

```bash
kubectl annotate application APP_NAME \
  -n argocd \
  argocd.argoproj.io/refresh=hard
```

---

## Sync Application

```bash
kubectl patch application APP_NAME \
  -n argocd \
  --type merge \
  -p '{"operation":{"sync":{"prune":true}}}'
```

---

# Best Practices

## Required

* Git-first changes
* Pull requests
* Documentation updates
* Small commits

---

## Avoid

* Manual resource changes
* Direct secret creation
* Untracked deployments

---

# Future Evolution

## Current

GitHub + ArgoCD

---

## Future

GitHub Actions
↓
Container Registry
↓
GitOps Promotion
↓
ArgoCD

---

## AWS

GitHub
↓
GitHub Actions
↓
ECR
↓
ArgoCD
↓
EKS

---

# Related Documentation

* platform-topology.md
* ci-cd.md
* security.md
* cluster-recovery.md
* backup-strategy.md

---

# Summary

GitOps is the operational foundation of NEXO-RAIDEN.

All infrastructure changes originate from Git, are reconciled by ArgoCD, and are continuously validated against the desired platform state.

