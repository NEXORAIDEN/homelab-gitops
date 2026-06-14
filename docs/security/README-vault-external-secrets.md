# NEXO-RAIDEN Platform

## Vault + External Secrets Integration

---

# Executive Summary

Vault and External Secrets provide centralized secret management for the NEXO-RAIDEN platform.

This architecture removes sensitive credentials from Git repositories and allows Kubernetes workloads to securely consume secrets from Vault.

The implementation is designed to support:

* Raspberry Pi 5 Homelab
* K3s Kubernetes
* ArgoCD GitOps
* Future AWS EKS Migration
* Future Multi-Cloud Deployments

---

# Business Value

Without centralized secret management:

```text
Git Repository
    ↓
Kubernetes Secret
    ↓
Application
```

Problems:

* Secrets committed to Git
* Difficult credential rotation
* No centralized auditing
* Compliance concerns
* Increased attack surface

With Vault and External Secrets:

```text
Vault
    ↓
External Secrets Operator
    ↓
Kubernetes Secret
    ↓
Application
```

Benefits:

* Centralized secret management
* Git remains secret-free
* Credential rotation support
* Security auditing
* Cloud-ready architecture
* Enterprise security best practices

---

# Architecture Overview

```text
┌─────────────────────┐
│      Vault          │
│ Secret Management   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ External Secrets    │
│ Operator            │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Kubernetes Secret   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Applications        │
│ Keycloak            │
│ PostgreSQL          │
│ Spring Boot Apps    │
└─────────────────────┘
```

---

# Platform Components

## Vault

Purpose:

Centralized secret storage.

Namespace:

```text
security
```

Runtime Component:

```text
vault-0
```

Responsibilities:

* Secret storage
* Secret retrieval
* Secret versioning
* Future secret rotation

---

## External Secrets Operator

Purpose:

Synchronizes Vault secrets into Kubernetes.

Namespace:

```text
security
```

Runtime Components:

```text
external-secrets
external-secrets-webhook
```

Responsibilities:

* Connect to Vault
* Retrieve secret values
* Create Kubernetes Secrets
* Keep secrets synchronized

---

# Repository Structure

```text
homelab-gitops
│
├── apps
│   ├── vault.yaml
│   ├── external-secrets.yaml
│   └── external-secrets-config.yaml
│
├── infrastructure
│   └── security
│       └── external-secrets
│           ├── cluster-secret-store.yaml
│           └── test-external-secret.yaml
│
└── docs
    └── security
        └── README-vault-external-secrets.md
```

---

# Configuration Files

## apps/vault.yaml

Purpose:

Deploys Vault through ArgoCD.

Why We Need It:

Without this application, Vault would not exist in the cluster.

Deployment Flow:

```text
Git
 ↓
ArgoCD
 ↓
Vault Helm Chart
 ↓
Vault Pod
```

---

## apps/external-secrets.yaml

Purpose:

Deploys External Secrets Operator.

Why We Need It:

Provides integration between Vault and Kubernetes.

Deployment Flow:

```text
Git
 ↓
ArgoCD
 ↓
External Secrets Operator
```

Important Decision:

```text
Pinned Version: 0.16.2
```

Reason:

Version 2.6.0 caused CRD annotation-size issues on K3s.

---

## apps/external-secrets-config.yaml

Purpose:

Deploys Vault integration configuration.

Why We Need It:

Allows ArgoCD to deploy:

* ClusterSecretStore
* ExternalSecret resources

Without this file:

```text
Vault works
BUT
Applications cannot consume secrets
```

---

## infrastructure/security/external-secrets/cluster-secret-store.yaml

Purpose:

Defines connection between Kubernetes and Vault.

Responsibilities:

* Vault endpoint
* Authentication method
* Secret engine path

Flow:

```text
ClusterSecretStore
       │
       ▼
Vault
```

---

## infrastructure/security/external-secrets/test-external-secret.yaml

Purpose:

Validation and integration test.

Responsibilities:

* Read secret from Vault
* Generate Kubernetes Secret

Flow:

```text
Vault Secret
      │
      ▼
ExternalSecret
      │
      ▼
Kubernetes Secret
```

---

# Installation History

## Initial Problems

Observed Issues:

```text
External Secrets CrashLoopBackOff
Missing SecretStore CRDs
Missing ClusterSecretStore CRDs
CRD Annotation Size Errors
```

Root Cause:

```text
External Secrets Chart 2.6.0
```

Resolution:

```text
Pinned Helm Chart
Version 0.16.2
```

Result:

```text
Stable Installation
Healthy Synchronization
```

---

# Verification Procedures

## Verify Vault

```bash
kubectl exec -n security vault-0 -- vault status
```

Expected:

```text
Initialized true
Sealed false
```

---

## Verify Pods

```bash
kubectl get pods -n security
```

Expected:

```text
vault-0
vault-agent-injector
external-secrets
external-secrets-webhook
```

---

## Verify ClusterSecretStore

```bash
kubectl get clustersecretstore
```

Expected:

```text
READY=True
```

---

## Verify External Secrets

```bash
kubectl get externalsecret -A
```

Expected:

```text
STATUS=SecretSynced
```

---

## Verify Secret Synchronization

```bash
kubectl get secret nexo-raiden-test-secret \
-n security \
-o jsonpath='{.data.username}' \
| base64 -d
```

Expected:

```text
nexo-test-user
```

---

# Troubleshooting

## Vault Sealed

Symptoms:

```text
vault-0 Not Ready
```

Fix:

```bash
vault operator unseal
```

Run with three valid unseal keys.

---

## External Secrets CrashLoopBackOff

Symptoms:

```text
external-secrets restarting
```

Common Causes:

* Missing CRDs
* Invalid Vault connection
* Authentication failure

Verify:

```bash
kubectl logs deployment/external-secrets -n security
```

---

## ClusterSecretStore Not Ready

Verify:

```bash
kubectl describe clustersecretstore nexo-raiden-vault
```

Check:

* Vault URL
* Authentication token
* Secret path

---

# Disaster Recovery

## Vault Restart

Verify:

```bash
kubectl exec -n security vault-0 -- vault status
```

If sealed:

```bash
vault operator unseal
```

---

## Rebuild External Secrets

```bash
kubectl delete application nexo-raiden-external-secrets -n argocd
```

ArgoCD will recreate it from Git.

---

# Future Improvements

## Phase 1

Current:

```text
Vault Token Authentication
```

Target:

```text
Vault Kubernetes Authentication
```

Benefits:

* No static token
* Better security
* Production-grade authentication

---

## Phase 2

Current:

```text
Manual Unseal
```

Target:

```text
Auto-Unseal
```

---

## Phase 3

Current:

```text
Local Vault Storage
```

Target:

```text
Encrypted Backup Strategy
```

---

# AWS Migration Path

Current Homelab:

```text
Vault
External Secrets
K3s
```

Future AWS:

```text
AWS Secrets Manager
External Secrets
Amazon EKS
```

External Secrets remains unchanged.

Only the backend secret provider changes.

---

# Current Status

```text
Vault                     Healthy
Vault Agent Injector      Healthy
External Secrets          Healthy
External Secrets Webhook  Healthy
ClusterSecretStore        Ready
ExternalSecret            SecretSynced
Kubernetes Secret Sync    Working
```

Status:

```text
Production-Ready Security Foundation Established
```
