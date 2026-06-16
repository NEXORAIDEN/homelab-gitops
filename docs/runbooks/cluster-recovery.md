# NEXO-RAIDEN Cluster Recovery Runbook

## Purpose

This runbook describes the complete recovery process for the NEXO-RAIDEN platform after catastrophic failure.

Examples:

* Raspberry Pi failure
* SD card corruption
* Ubuntu corruption
* K3s corruption
* Complete cluster loss

This runbook assumes the Git repository and backups are available.

---

# Platform Overview

Current platform:

* Ubuntu Server 24.04 LTS
* Raspberry Pi 5
* K3s
* ArgoCD
* Vault
* External Secrets
* cert-manager
* Keycloak
* PostgreSQL
* Ingress NGINX

---

# Recovery Objectives

## Recovery Time Objective (RTO)

Target:

```text
2–4 hours
```

---

## Recovery Point Objective (RPO)

Target:

```text
24 hours
```

Based on daily backups.

---

# Recovery Prerequisites

Required:

* Raspberry Pi replacement (or repaired node)
* Ubuntu Server image
* Network access
* GitHub access
* Vault backup
* PostgreSQL backup
* Vault unseal keys
* Vault root token

---

# Recovery Phase 1 – Host Recovery

## Install Ubuntu

Install:

```text
Ubuntu Server 24.04 LTS
```

---

## Configure Hostname

```bash
hostnamectl set-hostname nexo-raiden-lab-01
```

---

## Update System

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Install Base Tools

```bash
sudo apt install -y \
  curl \
  git \
  vim \
  jq \
  unzip
```

---

# Recovery Phase 2 – Kubernetes Recovery

## Install K3s

```bash
curl -sfL https://get.k3s.io | sh -
```

---

## Verify Cluster

```bash
kubectl get nodes
```

Expected:

```text
Ready
```

---

# Recovery Phase 3 – Repository Recovery

## Clone Repository

```bash
git clone <repository-url>
```

---

## Verify Repository

```bash
git status
```

Expected:

```text
working tree clean
```

---

# Recovery Phase 4 – ArgoCD Recovery

## Install ArgoCD

Apply installation manifests.

Verify:

```bash
kubectl get pods -n argocd
```

All pods must be running.

---

## Deploy Root Application

Apply:

```text
nexo-raiden-platform-root
```

---

## Verify Applications

```bash
kubectl get applications -n argocd
```

Expected:

```text
Synced
Healthy
```

---

# Recovery Phase 5 – Vault Recovery

## Deploy Vault

Verify:

```bash
kubectl get pods -n security
```

---

## Restore Vault Backup

Restore latest archive:

```text
~/backups/vault
```

Restore into:

```text
/vault/data
```

---

## Unseal Vault

```bash
vault operator unseal
```

Provide three keys.

---

## Validate Vault

```bash
vault status
```

Expected:

```text
Sealed false
```

---

# Recovery Phase 6 – PostgreSQL Recovery

## Deploy PostgreSQL

Verify:

```bash
kubectl get pods -n data
```

---

## Restore Database

```bash
gunzip backup.sql.gz
```

```bash
kubectl exec -i -n data deploy/nexo-raiden-postgresql -- \
  psql -U nexo_admin < backup.sql
```

---

## Verify Database

```bash
kubectl exec -it -n data deploy/nexo-raiden-postgresql -- \
  psql -U nexo_admin
```

---

# Recovery Phase 7 – Secret Validation

Verify:

```bash
kubectl get clustersecretstore
kubectl get externalsecret -A
```

Expected:

```text
READY=True
SecretSynced=True
```

---

# Recovery Phase 8 – Application Validation

Verify:

## Keycloak

```bash
kubectl get pods -n platform
```

Verify login:

```text
https://keycloak.nexo-raiden.local
```

---

## Certificates

```bash
kubectl get certificate -A
```

Expected:

```text
READY=True
```

---

## Ingress

```bash
kubectl get ingress -A
```

---

# Recovery Phase 9 – Platform Validation

Verify:

* ArgoCD Healthy
* Vault Healthy
* External Secrets Healthy
* PostgreSQL Healthy
* Keycloak Healthy
* TLS Working

---

# Recovery Checklist

* Ubuntu Installed
* K3s Running
* ArgoCD Running
* GitOps Restored
* Vault Restored
* Vault Unsealed
* PostgreSQL Restored
* Secrets Synced
* Keycloak Working
* TLS Working

---

# Future Improvements

Planned:

* Off-site Backups
* S3 Backups
* Automated Restore Validation
* AWS Recovery Environment
* Multi-Node K3s Cluster

---

# Related Documentation

* vault-backup.md
* vault-recovery.md
* postgresql-recovery.md
* backup-strategy.md
* platform-topology.md
* security.md
