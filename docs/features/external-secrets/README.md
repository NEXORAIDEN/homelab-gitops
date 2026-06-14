# External Secrets

## Purpose

External Secrets Operator (ESO) synchronizes secrets from external secret stores into Kubernetes.

In the NEXO-RAIDEN platform, External Secrets acts as the bridge between Vault and Kubernetes.

---

# Business Problem

Applications require Kubernetes Secrets.

Vault stores secrets externally.

Without External Secrets:

Application
↓
Custom Vault Client
↓
Complex Configuration

Problems:

- Application complexity
- Vendor coupling
- Difficult maintenance

---

# Solution

Vault
↓
External Secrets
↓
Kubernetes Secret
↓
Application

Applications continue using native Kubernetes Secrets.

---

# Current Secret Sources

## Vault

Current Provider:

HashiCorp Vault

---

# Current Consumers

## Keycloak

Consumes:

nexo-raiden-keycloak-secret

---

## PostgreSQL

Consumes:

nexo-raiden-postgresql-secret

---

# Benefits

- GitOps friendly
- Cloud portable
- Provider abstraction
- Secret synchronization
- Automatic refresh

---

# Current Status

| Component | Status |
|------------|---------|
| ESO Controller | Operational |
| ClusterSecretStore | Operational |
| Keycloak Secret Sync | Operational |
| PostgreSQL Secret Sync | Operational |
