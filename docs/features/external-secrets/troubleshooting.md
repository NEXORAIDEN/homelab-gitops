# External Secrets Troubleshooting

## CrashLoopBackOff

Symptoms:

external-secrets pod restarting

Cause:

Missing CRDs

---

## SecretStore Not Found

Error:

ClusterSecretStore missing

Verification:

kubectl get clustersecretstore

---

## SecretSynced False

Cause:

Vault unreachable

Invalid token

Invalid path

Verification:

kubectl describe externalsecret

---

## Missing CRDs

Error:

no matches for kind

SecretStore

ClusterSecretStore

Cause:

CRDs not installed

Resolution:

Verify:

kubectl get crd | grep secretstore

Expected:

clustersecretstores.external-secrets.io

secretstores.external-secrets.io

---

## CRD Annotation Too Large

Error:

metadata.annotations too long

Cause:

New ESO versions on K3s

Resolution:

Downgrade to:

0.16.2

Current Stable Version:

0.16.2

---

## Vault Integration Failure

Verify:

kubectl get clustersecretstore

Expected:

READY=True

---

## Secret Not Generated

Verify:

kubectl get externalsecret -A

STATUS:

SecretSynced=True
