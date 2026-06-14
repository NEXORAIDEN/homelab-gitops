# cert-manager

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
