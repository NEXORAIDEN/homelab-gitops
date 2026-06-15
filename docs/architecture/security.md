# NEXO-RAIDEN Security Architecture

## Document Information

| Attribute               | Value                   |
| ----------------------- | ----------------------- |
| Document Type           | Architecture Reference  |
| Platform                | NEXO-RAIDEN             |
| Environment             | Raspberry Pi 5 Homelab  |
| Kubernetes Distribution | K3s                     |
| Operating System        | Ubuntu Server 24.04 LTS |
| Security Domain         | Platform Security       |
| Last Updated            | 2026-06-14              |

---

# 1. Purpose

This document describes the complete security architecture of the NEXO-RAIDEN platform.

It explains:

* Identity Management
* Authentication
* Authorization
* Secret Management
* Certificate Management
* Secure Application Exposure
* Security Boundaries
* Operational Security Controls
* Future Cloud Security Evolution

The goal is to provide a repeatable, production-aligned security architecture that can evolve from a Raspberry Pi homelab into AWS and multi-cloud environments.

---

# 2. Security Objectives

The platform is designed around the following security objectives:

## Identity Centralization

All authentication must be managed through a centralized identity provider.

Current implementation:

* Keycloak

Future options:

* Keycloak HA
* AWS Cognito

---

## Secret Centralization

Secrets must never be stored:

* In Git repositories
* In Helm values files
* In Kubernetes manifests

Secrets are stored exclusively in Vault.

---

## Encryption

All externally exposed applications must use HTTPS.

Current implementation:

* cert-manager
* Self-signed certificates

Future:

* Let's Encrypt
* AWS ACM

---

## Least Privilege

Applications receive only the secrets they require.

No application directly accesses Vault.

---

## GitOps Security

Infrastructure changes must be:

* Version controlled
* Peer reviewable
* Reproducible
* Auditable

ArgoCD enforces the desired state.

---

# 3. Security Principles

The platform follows:

## Principle of Least Privilege

Applications receive only required access.

## Separation of Duties

Identity, certificates, secrets, networking and applications are separated.

## Defense in Depth

Multiple security layers exist:

* Identity
* Secrets
* TLS
* Namespace Isolation
* GitOps

## Zero Hardcoded Secrets

No passwords or credentials stored in Git.

---

# 4. Security Domains

The platform is divided into security domains.

## Identity Domain

Responsible for:

* Authentication
* Authorization
* Token issuance

Component:

* Keycloak

Namespace:

platform

---

## Secret Domain

Responsible for:

* Secret storage
* Secret distribution
* Secret lifecycle

Components:

* Vault
* External Secrets

Namespace:

security

---

## Certificate Domain

Responsible for:

* Certificate issuance
* TLS lifecycle
* Certificate renewal

Components:

* cert-manager
* ClusterIssuer

Namespace:

security

---

## Data Domain

Responsible for:

* Persistent storage
* User data
* Application data

Component:

* PostgreSQL

Namespace:

data

---

# 5. Trust Boundaries

Current architecture:

Developer
↓
MacBook
↓
Home Network
↓
Router
↓
Raspberry Pi Host
↓
Ubuntu Server
↓
K3s Cluster
↓
Namespaces
↓
Applications

Each layer represents a trust boundary.

Crossing a trust boundary requires explicit security controls.

---

# 6. Core Security Components

## Keycloak

Purpose:

Identity Provider.

Use Cases:

* User Authentication
* OIDC Provider
* SSO
* Role Management

Current Consumers:

* Administrators

Future Consumers:

* ArgoCD
* Grafana
* Spring Boot APIs

Files:

infrastructure/platform/keycloak/

---

## Vault

Purpose:

Central Secret Store.

Use Cases:

* Database Credentials
* Administrative Passwords
* API Keys
* Service Credentials

Files:

infrastructure/security/vault/

---

## External Secrets

Purpose:

Synchronize Vault secrets into Kubernetes.

Use Cases:

* Keycloak Credentials
* PostgreSQL Credentials
* Future Application Credentials

Files:

infrastructure/security/external-secrets/

---

## cert-manager

Purpose:

Automated certificate lifecycle.

Use Cases:

* HTTPS
* TLS Secret Creation
* Future Let's Encrypt Integration

Files:

infrastructure/security/cert-manager/

---

## Ingress NGINX

Purpose:

Platform entry point.

Use Cases:

* HTTPS Routing
* TLS Termination
* Reverse Proxy

Files:

apps/ingress-nginx.yaml

---

# 7. Namespace Security Model

security
├── Vault
├── External Secrets
├── cert-manager

platform
├── Keycloak
├── Ingress NGINX

data
└── PostgreSQL

Purpose:

Security isolation.

Benefits:

* Reduced blast radius
* Clear ownership
* Easier access control

---

# 8. Identity Architecture

User
↓
HTTPS
↓
Ingress NGINX
↓
Keycloak
↓
OIDC Token
↓
Application

Purpose:

Centralized identity management.

Benefits:

* Single Sign-On
* Central user lifecycle
* Consistent authentication

---

# 9. Authentication Flow

1. User accesses application
2. Application redirects to Keycloak
3. User authenticates
4. Keycloak validates credentials
5. Token issued
6. Application validates token
7. Access granted

Future:

* ArgoCD SSO
* Grafana SSO
* API Authentication

---

# 10. Secret Management Architecture

Vault
↓
ClusterSecretStore
↓
ExternalSecret
↓
Kubernetes Secret
↓
Application

Purpose:

Applications consume standard Kubernetes Secrets while Vault remains the source of truth.

Current Vault Paths:

nexo/platform/keycloak

nexo/data/postgresql

nexo/security/test

Current External Secrets:

platform/nexo-raiden-keycloak-secret

data/nexo-raiden-postgresql-secret

security/nexo-raiden-test-secret

---

# 11. Secret Lifecycle

Creation
↓
Vault
↓
Synchronization
↓
Kubernetes Secret
↓
Application Consumption
↓
Rotation
↓
Synchronization

Benefits:

* No Git exposure
* Central management
* Future rotation support

---

# 12. Certificate Architecture

Ingress
↓
Certificate Request
↓
cert-manager
↓
ClusterIssuer
↓
Certificate
↓
TLS Secret
↓
Ingress HTTPS

Current Resources:

ClusterIssuer:

nexo-raiden-selfsigned

Certificate:

keycloak-nexo-raiden-local-tls

Secret:

keycloak-nexo-raiden-local-tls

---

# 13. Network Exposure Model

External Access

22/TCP
SSH

30080/TCP
HTTP Ingress

30443/TCP
HTTPS Ingress

Internal Only

5432/TCP
PostgreSQL

8200/TCP
Vault

8080/TCP
Keycloak Service

Purpose:

Minimize attack surface.

---

# 14. Current Exposed Services

| Service          | Exposure |
| ---------------- | -------- |
| Keycloak         | HTTPS    |
| PostgreSQL       | Internal |
| Vault            | Internal |
| External Secrets | Internal |
| cert-manager     | Internal |

Security Principle:

Only ingress endpoints are exposed.

---

# 15. GitOps Security Model

Git
↓
ArgoCD
↓
Kubernetes

Benefits:

* Auditing
* Traceability
* Drift Detection
* Reproducibility

All infrastructure changes originate from Git.

---

# 16. Security Verification Procedures

Verify Vault:

kubectl exec -n security vault-0 -- vault status

Expected:

Sealed: false

---

Verify Secret Store:

kubectl get clustersecretstore

Expected:

READY=True

---

Verify External Secrets:

kubectl get externalsecret -A

Expected:

READY=True

---

Verify Certificates:

kubectl get certificate -A

Expected:

READY=True

---

Verify HTTPS:

curl -k -I https://keycloak.nexo-raiden.local:30443

Expected:

HTTP/2 302

---

# 17. Backup and Recovery

Critical Assets:

* Vault Data
* PostgreSQL Data
* Git Repository
* TLS Certificates

Recovery Priority:

1. Git Repository
2. Vault
3. PostgreSQL
4. Keycloak
5. Applications

---

# 18. Known Risks

Current Limitations:

* Self-signed certificates
* Manual Vault unseal
* Root token bootstrap
* No NetworkPolicies
* No Falco
* No OPA/Gatekeeper
* No Trivy scanning

---

# 19. Security Roadmap

Phase 1

✓ Vault
✓ External Secrets
✓ Keycloak
✓ cert-manager

Phase 2

□ NetworkPolicies
□ ArgoCD SSO
□ Grafana SSO

Phase 3

□ Trivy
□ Falco
□ OPA/Gatekeeper
□ Cosign

Phase 4

□ HCP Vault
□ AWS IAM Integration
□ ACM
□ Route53

---

# 20. AWS Security Mapping

| Current        | AWS            |
| -------------- | -------------- |
| K3s            | EKS            |
| Vault          | HCP Vault      |
| Self-Signed    | ACM            |
| Local DNS      | Route53        |
| PostgreSQL Pod | RDS PostgreSQL |
| NodePort       | ALB            |

---

# 21. Security Architecture Summary

The NEXO-RAIDEN platform implements a layered security model consisting of:

* Identity Management
* Secret Management
* Certificate Management
* Secure Networking
* GitOps Governance

This architecture serves as the foundation for future observability, messaging, application and AI workloads while maintaining cloud portability and security best practices.
# Security Architecture

## Purpose

The NEXO-RAIDEN security architecture defines how identity, secrets, certificates, ingress exposure, and platform access are managed across the Kubernetes platform.

The NEXO-RAIDEN Security Architecture provides:

- Authentication
- Authorization
- Secret Management
- Certificate Management
- Secure Service Communication

---

# Components

Keycloak
Vault
External Secrets
cert-manager
Ingress NGINX

---

# Security Domains

Identity
Secrets
Certificates
Network Security

---

## Security Objectives

- Keep secrets out of Git
- Centralize identity management
- Encrypt browser-to-platform traffic
- Use GitOps for auditable configuration
- Separate workloads by namespace
- Minimize externally exposed ports
- Prepare for AWS and multi-cloud security patterns

## Trust Boundaries

```text
Developer Laptop
↓
Local Network
↓
Raspberry Pi Host
↓
K3s Cluster
↓
Namespaces
↓
Applications
