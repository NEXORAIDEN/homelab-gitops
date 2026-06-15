# cert-manager Architecture

## Current Architecture

Ingress
    │
    ▼
Certificate Request
    │
    ▼
cert-manager
    │
    ▼
ClusterIssuer
    │
    ▼
TLS Secret
    │
    ▼
Ingress NGINX

---

# Current Flow

Keycloak Ingress

↓

cert-manager Annotation

↓

ClusterIssuer

↓

Certificate

↓

TLS Secret

↓

Ingress HTTPS

---

# Kubernetes Resources

ClusterIssuer

Purpose:

Certificate authority definition

---

Certificate

Purpose:

Certificate request

---

Secret

Purpose:

TLS storage

---

# Current ClusterIssuer

Name:

nexo-raiden-selfsigned

Type:

SelfSigned# cert-manager

## Purpose

cert-manager automates certificate management inside Kubernetes.

## Why We Need It

Ingress traffic should use HTTPS instead of plain HTTP.

## Current Goal

Enable local TLS for:

- Keycloak
- Future ArgoCD
- Future Grafana

## Namespace

security

## Files

| File | Purpose |
|---|---|
| apps/cert-manager.yaml | ArgoCD app for cert-manager |
| apps/cert-manager-issuers.yaml | ArgoCD app for issuers |
| infrastructure/security/cert-manager/cluster-issuer.yaml | Self-signed issuer |

## Local Lab Strategy

Use self-signed certificates first.

Future cloud strategy:

- AWS ACM
- cert-manager with DNS-01
- Route53
- Let’s Encrypt
