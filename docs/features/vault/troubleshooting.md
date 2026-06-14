# Vault Troubleshooting

## Vault Sealed

Symptoms:

Readiness probe failing

Vault status:

Sealed: true

Resolution:

Unseal Vault

3 of 5 keys required

---

## Vault Not Initialized

Symptoms:

Initialized: false

Resolution:

vault operator init

---

## External Secrets Failing

Symptoms:

SecretSynced=False

Verification:

kubectl get clustersecretstore

kubectl get externalsecret -A

---

## CrashLoopBackOff

Possible Causes:

Vault sealed

PVC issue

Configuration issue

Verification:

kubectl logs -n security vault-0

kubectl describe pod vault-0

---

## Secret Not Found

Cause:

Incorrect path

Example:

nexo/platform/keycloak

Verify:

vault kv list nexo
