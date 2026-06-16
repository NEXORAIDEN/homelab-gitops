# ArgoCD Recovery Runbook

## Purpose

ArgoCD is the GitOps control plane for the NEXO-RAIDEN platform.

If ArgoCD is unhealthy, GitOps reconciliation stops. Existing workloads may continue running, but new changes will not deploy automatically.

## Impact If ArgoCD Fails

- Git changes are not synchronized
- Drift detection stops
- Self-healing stops
- New platform services are not deployed
- Existing workloads may continue running

## Verify ArgoCD Pods

```bash
kubectl get pods -n argocd
