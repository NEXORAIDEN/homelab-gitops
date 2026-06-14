# Keycloak

## Purpose

Identity and Access Management.

## Business Value

Provides:

- Authentication
- Authorization
- Single Sign-On
- OIDC
- OAuth2

## Why We Need It

Every modern platform needs centralized identity management.

Without Keycloak:

Applications manage users individually.

With Keycloak:

Applications trust a central Identity Provider.

## Namespace

platform

# Keycloak

## Purpose

Keycloak provides Identity and Access Management (IAM) capabilities for the NEXO-RAIDEN platform.

It serves as the central authentication and authorization system for all platform services.

---

# Business Problem

Modern applications require:

- Authentication
- Authorization
- Single Sign-On (SSO)
- Identity Federation
- OAuth2
- OpenID Connect (OIDC)

Implementing these capabilities individually inside every application is complex and error-prone.

---

# Solution

Keycloak centralizes identity management.

Applications delegate authentication to Keycloak.

Benefits:

- Centralized user management
- Role-based access control
- OAuth2 support
- OIDC support
- SSO
- Federation capabilities

---

# Current Platform Role

Keycloak currently provides:

- Platform Identity Provider
- Administrative Authentication

Future:

- ArgoCD SSO
- Grafana SSO
- Spring Boot OIDC Authentication
- API Gateway Authentication

---

# Current Status

| Component | Status |
|------------|---------|
| Deployment | Operational |
| PostgreSQL Backend | Operational |
| TLS | Operational |
| Vault Secrets | Operational |
| External Secrets | Operational |
| Ingress | Operational |## Files

| File | Purpose |
|--------|---------|
| apps/keycloak.yaml | ArgoCD Application |
| infrastructure/platform/keycloak/* | Keycloak manifests |

## Current Consumers

- Future Spring Boot Services
- Grafana
- ArgoCD (future)
- APIs

## Verification

```bash
kubectl get pods -n platform
