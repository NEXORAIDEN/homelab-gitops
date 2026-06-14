# External Secrets Architecture

## Secret Flow

Vault
    │
    ▼
ClusterSecretStore
    │
    ▼
ExternalSecret
    │
    ▼
Kubernetes Secret
    │
    ▼
Application

---

# Current Architecture

Vault

nexo/platform/keycloak

↓

ExternalSecret

nexo-raiden-keycloak-secret

↓

Kubernetes Secret

nexo-raiden-keycloak-secret

↓

Keycloak

---

# PostgreSQL Flow

Vault

nexo/data/postgresql

↓

ExternalSecret

nexo-raiden-postgresql-secret

↓

Kubernetes Secret

nexo-raiden-postgresql-secret

↓

PostgreSQL

---

# Kubernetes Resources

ClusterSecretStore

Purpose:

Vault connection definition

---

ExternalSecret

Purpose:

Synchronization configuration

---

Secret

Purpose:

Application consumption

---

# Refresh Flow

Vault Change
↓
Refresh Interval
↓
External Secrets Sync
↓
Kubernetes Secret Updated
