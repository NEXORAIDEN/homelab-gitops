# Vault Architecture

## Current Architecture

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

# Secret Flow Example

Vault

nexo/platform/keycloak

↓

External Secret

nexo-raiden-keycloak-secret

↓

Kubernetes Secret

nexo-raiden-keycloak-secret

↓

Keycloak Deployment

---

# Current Secret Paths

nexo/

├── platform/
│   └── keycloak
│
├── data/
│   └── postgresql
│
└── security/
    └── test

---

# Kubernetes Components

Namespace:

security

Resources:

vault-0

vault-agent-injector

---

# Authentication Model

Current:

Root Token

Future:

Kubernetes Auth Method

Service Accounts

Vault Policies

---

# Security Boundaries

Applications
    │
    ▼
Kubernetes Secret

Applications never directly access Vault.
