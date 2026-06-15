# NEXO-RAIDEN GitOps Architecture

## Document Information

| Attribute     | Value                  |
| ------------- | ---------------------- |
| Document Type | Architecture Reference |
| Domain        | GitOps                 |
| Platform      | NEXO-RAIDEN            |
| GitOps Engine | ArgoCD                 |
| Repository    | homelab-gitops         |
| Kubernetes    | K3s                    |
| Environment   | Raspberry Pi 5         |

---

# 1. Purpose

This document describes the GitOps architecture of the NEXO-RAIDEN platform.

It explains:

* Desired state management
* Application deployment
* Infrastructure deployment
* Drift detection
* Self-healing
* Repository structure
* ArgoCD architecture
* Future multi-cluster strategy

---

# 2. GitOps Objectives

The platform follows GitOps principles.

Goals:

* Declarative infrastructure
* Auditable changes
* Automated deployments
* Self-healing systems
* Reproducible environments
* Disaster recovery

---

# 3. What Is GitOps?

GitOps is an operational model where Git becomes the source of truth.

Traditional Operations:

Engineer
↓
kubectl apply
↓
Cluster

Problems:

* Drift
* Manual changes
* Limited traceability

GitOps:

Git
↓
ArgoCD
↓
Cluster

Benefits:

* Traceability
* Auditability
* Repeatability
* Automation

---

# 4. Source of Truth

Primary Source:

Git Repository

Repository:

homelab-gitops

All infrastructure must originate from Git.

Direct cluster modifications are considered configuration drift.

---

# 5. GitOps Control Plane

Current Control Plane:

ArgoCD

Namespace:

argocd

Responsibilities:

* Reconciliation
* Drift Detection
* Automated Sync
* Self-Healing
* Application Management

---

# 6. Repository Architecture

Current Repository Structure

homelab-gitops/

├── apps/
├── clusters/
├── infrastructure/
├── docs/

Purpose:

Clear separation of responsibilities.

---

# 7. apps Directory

Purpose:

Application definitions.

Contains:

apps/

├── ingress-nginx.yaml
├── vault.yaml
├── external-secrets.yaml
├── cert-manager.yaml
├── keycloak.yaml
├── postgresql.yaml

Responsibilities:

* Helm deployments
* ArgoCD applications
* Platform services

---

# 8. clusters Directory

Purpose:

Cluster-specific configuration.

Current:

clusters/lab-01/

Contains:

* AppProjects
* Root Applications
* Cluster configuration

Benefits:

Supports future multi-cluster environments.

---

# 9. infrastructure Directory

Purpose:

Workload manifests.

Examples:

infrastructure/

├── platform/
├── data/
├── security/

Contains:

* Deployments
* Services
* Ingresses
* ExternalSecrets
* Certificates

---

# 10. Documentation Directory

Purpose:

Architecture and operational documentation.

Contains:

* Architecture Guides
* Runbooks
* Feature Documentation
* Recovery Procedures

---

# 11. ArgoCD Architecture

GitHub
↓
ArgoCD
↓
Kubernetes API
↓
Cluster Resources

ArgoCD continuously reconciles the cluster.

---

# 12. Application Hierarchy

Root Application

↓

Project Applications

↓

Platform Applications

Current Example:

Root
↓
Project
↓
Vault
↓
External Secrets
↓
Keycloak
↓
PostgreSQL

---

# 13. Application Lifecycle

Developer
↓
Git Commit
↓
Git Push
↓
GitHub
↓
ArgoCD Detects Change
↓
Sync
↓
Cluster Updated

---

# 14. Drift Detection

Desired State

↓

Git

Actual State

↓

Cluster

ArgoCD compares both continuously.

If differences exist:

OutOfSync

Status is generated.

---

# 15. Self-Healing

If a resource is manually modified:

Cluster
↓
Drift Detected
↓
ArgoCD
↓
Reconciliation
↓
Desired State Restored

Benefits:

* Consistency
* Reliability
* Auditability

---

# 16. Security Model

GitOps Security Controls

* Source Repository Restrictions
* Namespace Restrictions
* Project Boundaries
* RBAC
* Immutable History

Current AppProject:

nexo-raiden-platform

Controls:

* Allowed repositories
* Allowed namespaces
* Allowed cluster resources

---

# 17. Deployment Strategy

Current:

Automatic Sync

Automatic Prune

Self Heal

Benefits:

* Reduced operations effort
* Faster recovery
* Consistent deployments

---

# 18. Operational Procedures

Verify Applications

kubectl get applications -n argocd

Expected:

Synced
Healthy

---

Verify Application

kubectl get application APP_NAME -n argocd

---

Inspect Application

kubectl describe application APP_NAME -n argocd

---

Check ArgoCD Pods

kubectl get pods -n argocd

---

# 19. Disaster Recovery

Git is the recovery source.

Recovery Flow:

New Cluster
↓
Install ArgoCD
↓
Deploy Root Application
↓
Automatic Rebuild

Benefits:

Infrastructure rebuild within minutes.

---

# 20. Current Applications

Infrastructure Layer

* Ingress NGINX
* Vault
* External Secrets
* cert-manager

Platform Layer

* Keycloak

Data Layer

* PostgreSQL

---

# 21. Known Limitations

Current:

* Single Cluster
* Local GitHub Dependency
* No Multi-Cluster Management
* No Progressive Delivery

---

# 22. Future GitOps Roadmap

Phase 1

✓ ArgoCD
✓ GitOps Repository
✓ Auto Sync
✓ Self Heal

Phase 2

□ ArgoCD SSO
□ HTTPS Access
□ RBAC Hardening

Phase 3

□ Multi-Cluster GitOps
□ Progressive Delivery
□ Blue/Green Deployments

Phase 4

□ AWS EKS
□ Multi-Cloud Deployments

---

# 23. AWS Mapping

| Current      | AWS                |
| ------------ | ------------------ |
| K3s          | EKS                |
| ArgoCD       | ArgoCD             |
| GitHub       | GitHub             |
| NodePort     | ALB                |
| Raspberry Pi | AWS Infrastructure |

---

# 24. GitOps Architecture Summary

The NEXO-RAIDEN platform uses GitOps as the primary operational model.

Git serves as the source of truth.

ArgoCD enforces desired state.

This provides:

* Consistency
* Automation
* Recoverability
* Auditability
* Cloud Portability

and forms the foundation of all platform operations.
