# External Secrets

## Purpose

Synchronize secrets from Vault to Kubernetes.

## Why We Need It

Applications expect Kubernetes Secrets.

External Secrets bridges:

Vault → Kubernetes

## Namespace

security

## Files

apps/external-secrets.yaml

infrastructure/security/external-secrets/cluster-secret-store.yaml

infrastructure/security/external-secrets/test-external-secret.yaml

## Verification

kubectl get clustersecretstore

kubectl get externalsecret -A

## Dependencies

- Vault
- CRDs

## Known Issues

Chart 2.6.0 caused CRD annotation issues.

Pinned version:

0.16.2
