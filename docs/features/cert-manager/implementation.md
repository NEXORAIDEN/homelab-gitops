# cert-manager Implementation

## ArgoCD Applications

apps/cert-manager.yaml

Purpose:

Deploy cert-manager.

---

apps/cert-manager-issuers.yaml

Purpose:

Deploy issuers.

---

# Namespace

security

---

# Files

## apps/cert-manager.yaml

Deploys:

- Controller
- Webhook
- CA Injector

Repository:

https://charts.jetstack.io

---

## infrastructure/security/cert-manager/cluster-issuer.yaml

Purpose:

Create ClusterIssuer.

Current:

SelfSigned

---

## infrastructure/platform/keycloak/ingress.yaml

Purpose:

Request TLS certificate.

Annotation:

cert-manager.io/cluster-issuer

TLS Secret:

keycloak-nexo-raiden-local-tls

---

# Verification

kubectl get pods -n security

kubectl get clusterissuer

kubectl get certificate -A
