# Vault Init and Unseal Runbook

## Purpose

Vault starts sealed after deployment or restart. It must be initialized once and unsealed after each restart unless auto-unseal is configured.

## Check Status

```bash
kubectl exec -n security vault-0 -- vault status
