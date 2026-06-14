# External Secrets Implementation

## ArgoCD Application

File:

apps/external-secrets.yaml

Purpose:

Deploy External Secrets Operator.

---

# Namespace

security

---

# Files

## apps/external-secrets.yaml

Purpose:

Deploy ESO Helm Chart.

Repository:

https://charts.external-secrets.io

---

## apps/external-secrets-config.yaml

Purpose:

Deploy configuration resources.

---

## infrastructure/security/external-secrets/

Contains:

- ClusterSecretStore
- Test ExternalSecret
- Future Stores

---

# Current Resources

ClusterSecretStore

Name:

nexo-raiden-vault

Purpose:

Connect ESO to Vault

---

ExternalSecret

Name:

nexo-raiden-test-secret

Purpose:

Connectivity verification

---

ExternalSecret

Name:

nexo-raiden-keycloak-secret

Purpose:

Keycloak credential synchronization

---

ExternalSecret

Name:

nexo-raiden-postgresql-secret

Purpose:

PostgreSQL credential synchronization

---

# Verification

kubectl get clustersecretstore

kubectl get externalsecret -A

kubectl get secret -A
