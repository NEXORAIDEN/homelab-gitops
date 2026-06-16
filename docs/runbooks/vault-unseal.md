# Vault Unseal Runbook

## Purpose

Vault stores platform secrets for NEXO-RAIDEN.

After a restart, reboot, or pod recreation, Vault may start in a sealed state. When sealed, Vault cannot serve secrets to External Secrets Operator.

This runbook explains how to verify Vault state and unseal it safely.

## Impact If Vault Is Sealed

If Vault is sealed:

- External Secrets cannot refresh secrets
- New Kubernetes Secrets cannot be generated
- Secret rotation is blocked
- Applications may continue running with existing Kubernetes Secrets
- New deployments depending on Vault may fail

## Verify Vault Pod

```bash
kubectl get pods -n security | grep vault
