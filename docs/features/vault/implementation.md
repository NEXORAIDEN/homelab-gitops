# Vault Implementation

## ArgoCD Application

File:

apps/vault.yaml

Purpose:

Deploy Vault via Helm.

---

# Namespace

security

---

# Files

## apps/vault.yaml

Purpose:

Deploy Vault Helm Chart.

Repository:

https://helm.releases.hashicorp.com

---

## infrastructure/security/vault/

Purpose:

Vault-specific manifests.

---

# Vault Configuration

Storage:

File Storage

Current Platform:

Raspberry Pi 5

---

# Initialization

Command:

kubectl exec -it -n security vault-0 -- vault operator init

Purpose:

Initialize Vault.

Output:

- Unseal Keys
- Root Token

---

# Unseal Process

Command:

vault operator unseal

Required:

3 of 5 keys

---

# Secret Creation

Example:

vault kv put \
nexo/platform/keycloak \
KC_DB_USERNAME=xxx

---

# Verification

kubectl exec -n security vault-0 -- vault status

Expected:

Sealed: false
