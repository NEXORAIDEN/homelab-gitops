# Keycloak Architecture

## Current Architecture

Browser
    │
 HTTPS
    ▼
Ingress NGINX
    │
 HTTP
    ▼
Keycloak
    │
 JDBC
    ▼
PostgreSQL

---

# Secret Flow

Vault
    │
    ▼
External Secrets
    │
    ▼
Kubernetes Secret
    │
    ▼
Keycloak

---

# Kubernetes Resources

Deployment

nexo-raiden-keycloak

Namespace:

platform

---

Service

nexo-raiden-keycloak

Port:

8080

---

Ingress

nexo-raiden-keycloak

Host:

keycloak.nexo-raiden.local

---

Certificate

keycloak-nexo-raiden-local-tls

Issued By:

nexo-raiden-selfsigned

---

# Authentication Flow

User
 ↓
Keycloak Login
 ↓
Identity Verification
 ↓
Token Issued
 ↓
Application Access

---

# Future Authentication Architecture

User
 ↓
Keycloak
 ↓
OIDC Token
 ↓
Spring Boot Services
 ↓
Role Validation
