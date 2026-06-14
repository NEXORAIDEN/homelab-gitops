# Keycloak Implementation

## ArgoCD Application

File:

apps/keycloak.yaml

Purpose:

Deploy Keycloak through GitOps.

---

# Namespace

platform

---

# Files

## deployment.yaml

Location:

infrastructure/platform/keycloak/deployment.yaml

Purpose:

Creates Keycloak deployment.

Important Variables:

KC_DB

KC_DB_URL

KC_DB_USERNAME

KC_DB_PASSWORD

KEYCLOAK_ADMIN

KEYCLOAK_ADMIN_PASSWORD

KC_HOSTNAME

KC_PROXY_HEADERS

KC_HTTP_ENABLED

---

## service.yaml

Location:

infrastructure/platform/keycloak/service.yaml

Purpose:

Creates internal service.

Port:

8080

---

## ingress.yaml

Location:

infrastructure/platform/keycloak/ingress.yaml

Purpose:

Creates HTTPS endpoint.

Host:

keycloak.nexo-raiden.local

TLS Secret:

keycloak-nexo-raiden-local-tls

---

## external-secret.yaml

Location:

infrastructure/platform/keycloak/external-secret.yaml

Purpose:

Retrieves credentials from Vault.

---

# Database Connection

Endpoint:

nexo-raiden-postgresql.data.svc.cluster.local

Port:

5432

Database:

nexo_platform

---

# Verification

kubectl get pods -n platform

kubectl get ingress -n platform

kubectl get certificate -n platform

kubectl get externalsecret -n platform
