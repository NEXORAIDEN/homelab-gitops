# PostgreSQL Architecture

## Current Architecture

Keycloak
    │
    ▼
PostgreSQL Service
    │
    ▼
PostgreSQL Pod
    │
    ▼
Persistent Volume Claim

---

# Secret Flow

Vault
    │
    ▼
External Secret
    │
    ▼
Kubernetes Secret
    │
    ▼
PostgreSQL

---

# Storage Architecture

Application
    │
    ▼
PVC
    │
    ▼
Node Storage

Current Environment:

Raspberry Pi 5

---

# Kubernetes Resources

Deployment

nexo-raiden-postgresql

---

Service

nexo-raiden-postgresql

---

PersistentVolumeClaim

postgresql-pvc

---

ExternalSecret

nexo-raiden-postgresql-secret
