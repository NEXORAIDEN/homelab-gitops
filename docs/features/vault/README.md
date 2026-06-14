# Vault

## Purpose

HashiCorp Vault is the central secret management system of the NEXO-RAIDEN platform.

It provides:

- Secret Storage
- Secret Versioning
- Secret Auditing
- Secret Rotation
- Centralized Credential Management

---

# Business Problem

Without Vault:

Application
↓
Hardcoded Secret
↓
Git Repository

Problems:

- Secrets in Git
- No rotation
- No audit trail
- Security risk

---

# Solution

Vault stores secrets centrally.

Applications never access Vault directly.

Secrets are delivered through:

Vault
↓
External Secrets
↓
Kubernetes Secret
↓
Application

---

# Current Consumers

## Keycloak

Secrets:

- KC_DB_USERNAME
- KC_DB_PASSWORD
- KEYCLOAK_ADMIN
- KEYCLOAK_ADMIN_PASSWORD

---

## PostgreSQL

Secrets:

- POSTGRES_DB
- POSTGRES_USER
- POSTGRES_PASSWORD

---

# Benefits

- Centralized secrets
- Secret versioning
- GitOps compatibility
- Cloud portability
- Enterprise security model

---

# Current Status

| Component | Status |
|------------|---------|
| Vault Server | Operational |
| Vault Storage | Operational |
| KV Store | Operational |
| External Secrets Integration | Operational |
| Keycloak Integration | Operational |
| PostgreSQL Integration | Operational |
