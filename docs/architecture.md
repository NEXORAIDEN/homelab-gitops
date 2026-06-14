# NEXO-RAIDEN Platform Architecture

## Current State

The current platform runs on a Raspberry Pi 5 with Ubuntu 24.04, K3s, ArgoCD, Ingress NGINX, PostgreSQL, Keycloak, Vault, and External Secrets.

## Core Principles

- GitOps-first
- Infrastructure as Code
- Least privilege
- Namespace isolation
- Resource guardrails
- Secrets externalization
- Documentation as deliverable

## Namespaces

- `argocd`
- `platform`
- `security`
- `storage`
- `data`
- `monitoring`
- `ai`
- `applications`

## Current Service Flow

```text
Browser
→ Local DNS / hosts file
→ Ingress NGINX
→ Keycloak
→ PostgreSQL
