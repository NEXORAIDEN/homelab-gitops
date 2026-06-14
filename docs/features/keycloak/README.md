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

## Files

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
