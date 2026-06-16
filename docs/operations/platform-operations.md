# Platform Operations Guide

## Document Information

| Attribute      | Value                                          |
| -------------- | ---------------------------------------------- |
| Document Name  | Platform Operations Guide                      |
| Platform       | NEXO-RAIDEN                                    |
| Classification | Operations                                     |
| Audience       | Platform Engineers, DevOps Engineers           |
| Purpose        | Define operational procedures for the platform |

---

# 1. Executive Summary

This document describes the day-to-day operational procedures required to maintain the NEXO-RAIDEN platform.

The objective is to ensure:

* Availability
* Reliability
* Security
* Recoverability
* Operational Consistency

---

# 2. Operational Philosophy

The platform follows:

GitOps
+
Automation
+
Documentation

Every operational action should be:

* Repeatable
* Auditable
* Documented

---

# 3. Daily Operations Checklist

## Infrastructure

Verify:

```bash
kubectl get nodes
```

Expected:

```text
Ready
```

---

## Platform Services

Verify:

```bash
kubectl get pods -A
```

Check:

* Vault
* Keycloak
* PostgreSQL
* ArgoCD
* cert-manager
* External Secrets

---

## GitOps Health

Verify:

```bash
kubectl get applications -n argocd
```

Expected:

```text
Synced
Healthy
```

---

## Storage Health

Verify:

```bash
kubectl get pvc -A
```

Expected:

```text
Bound
```

---

## Certificate Health

Verify:

```bash
kubectl get certificates -A
```

Expected:

```text
READY=True
```

---

# 4. Weekly Operations Checklist

## Backup Validation

Verify:

```bash
ls -lh ~/backups/postgresql
ls -lh ~/backups/vault
```

---

## Capacity Review

Check:

* CPU
* Memory
* Disk Usage

---

## Certificate Review

Verify expiration dates.

---

## Documentation Review

Confirm documentation reflects current platform state.

---

# 5. Monthly Operations Checklist

## Recovery Review

Review:

* Cluster Recovery
* PostgreSQL Recovery
* Vault Recovery

---

## Security Review

Review:

* Users
* Roles
* Secrets
* Certificates

---

## Dependency Review

Review:

* ArgoCD Applications
* Platform Dependencies

---

# 6. Platform Health Checks

## Kubernetes

```bash
kubectl cluster-info
```

---

## ArgoCD

```bash
kubectl get applications -n argocd
```

---

## Vault

```bash
kubectl get pods -n security
```

---

## PostgreSQL

```bash
kubectl get pods -n data
```

---

## Keycloak

```bash
kubectl get pods -n platform
```

---

# 7. Incident Management

## Incident Levels

### Critical

Examples:

* Cluster Failure
* Vault Failure
* PostgreSQL Failure

Target Response:

15 Minutes

---

### High

Examples:

* Keycloak Failure
* Certificate Failure

Target Response:

30 Minutes

---

### Medium

Examples:

* Dashboard Failure

Target Response:

4 Hours

---

# 8. Change Management

## Change Process

Engineer
↓
Git Commit
↓
Pull Request
↓
Review
↓
GitHub Actions
↓
ArgoCD
↓
Deployment

---

## Forbidden Actions

Avoid:

```text
kubectl apply
manual secrets
direct production modifications
```

Unless part of emergency recovery.

---

# 9. Operational Metrics

Monitor:

* Node Availability
* Pod Health
* Certificate Status
* Backup Status
* Storage Usage
* Authentication Availability

---

# 10. Platform Responsibilities

## Platform Engineer

Responsible for:

* Kubernetes
* ArgoCD
* GitOps

---

## Security Engineer

Responsible for:

* Vault
* Keycloak
* Certificates

---

## Application Engineer

Responsible for:

* Services
* APIs
* Messaging

---

# 11. Future Operations

Future additions:

* Alertmanager
* On-call Procedures
* Incident Reviews
* SLO Tracking
* Capacity Forecasting

---

# 12. Summary

This document provides the operational foundation for running the NEXO-RAIDEN platform in a reliable, secure, and repeatable manner.
