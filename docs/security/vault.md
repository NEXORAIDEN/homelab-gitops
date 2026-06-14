# Vault

## Purpose

Centralized secret management.

## Why We Need It

Without Vault:

- Secrets stored in Git
- Difficult rotation
- No auditing

With Vault:

- Centralized secrets
- Secret rotation
- Auditability

## Namespace

security

## Files

apps/vault.yaml

## Verification

kubectl get pods -n security

kubectl exec -n security vault-0 -- vault status

## Dependencies

- K3s
- ArgoCD

## Future

- Auto-unseal
- AWS Secrets Manager migration
