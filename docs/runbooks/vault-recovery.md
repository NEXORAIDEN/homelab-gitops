# Vault Recovery Runbook

## Purpose

This runbook documents the complete recovery procedure for HashiCorp Vault within the NEXO-RAIDEN platform.

Vault is the central secret management system and serves as the source of truth for:

* PostgreSQL credentials
* Keycloak credentials
* Future application credentials
* Future API keys
* Future cloud credentials

Loss of Vault may impact the entire platform.

---

# Recovery Scenarios

## Scenario 1

Vault Pod Failure

Examples:

* Vault container crash
* Vault restart
* Node reboot

Expected Recovery:

* Restart pod
* Unseal Vault
* Verify External Secrets

---

## Scenario 2

Vault PVC Loss

Examples:

* PVC deletion
* Storage corruption
* SD card corruption

Expected Recovery:

* Recreate Vault
* Restore backup
* Unseal Vault
* Verify secrets

---

## Scenario 3

Full Cluster Loss

Examples:

* Raspberry Pi failure
* SD card failure
* K3s corruption

Expected Recovery:

* Rebuild cluster
* Reinstall ArgoCD
* Restore Vault
* Restore PostgreSQL

---

# Dependencies

Vault depends on:

* Kubernetes
* Persistent Storage
* Vault PVC

Applications depending on Vault:

* External Secrets
* Keycloak
* PostgreSQL
* Future Platform Services

---

# Current Storage Backend

Vault currently uses:

```hcl
storage "file" {
  path = "/vault/data"
}
```

Location:

```text
/vault/data
```

PVC:

```text
security/data-vault-0
```

Capacity:

```text
5Gi
```

---

# Backup Location

Current backup directory:

```text
/home/nexo-raiden/backups/vault
```

Example:

```text
vault-file-storage-20260615-183308.tar.gz
```

---

# Recovery Procedure

## Step 1

Verify Vault Deployment

```bash
kubectl get pods -n security
```

Expected:

```text
vault-0
```

---

## Step 2

Verify Vault Status

```bash
kubectl exec -n security vault-0 -- vault status
```

---

## Step 3

Restore Vault Data

Copy backup archive.

Example:

```bash
tar xzf vault-file-storage-YYYYMMDD-HHMMSS.tar.gz
```

Restore data into:

```text
/vault/data
```

---

## Step 4

Restart Vault

```bash
kubectl rollout restart statefulset/vault -n security
```

Wait:

```bash
kubectl rollout status statefulset/vault -n security
```

---

## Step 5

Unseal Vault

Three separate unseal keys are required.

```bash
kubectl exec -it -n security vault-0 -- vault operator unseal
```

Repeat three times.

---

## Step 6

Verify Vault Health

```bash
kubectl exec -n security vault-0 -- vault status
```

Expected:

```text
Initialized true
Sealed false
```

---

## Step 7

Verify External Secrets

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

## Step 8

Verify Application Secrets

```bash
kubectl get secret -A
```

Verify:

* PostgreSQL Secret
* Keycloak Secret

---

# Validation Checklist

* Vault Running
* Vault Unsealed
* ClusterSecretStore Valid
* External Secrets Synced
* PostgreSQL Credentials Available
* Keycloak Credentials Available

---

# Recovery Time Objective (RTO)

Target:

```text
< 30 minutes
```

---

# Recovery Point Objective (RPO)

Current:

```text
24 hours
```

Based on daily backups.

---

# Future Improvements

Future roadmap:

* Raft Storage
* Automated Snapshots
* S3 Backup
* AWS Migration
* Auto-Unseal

---

# Related Documentation

* vault-unseal.md
* vault-backup.md
* backup-strategy.md
* cluster-recovery.md
* security.md
* platform-topology.md
# Vault Recovery

## Purpose

Restore Vault after:

- SD card failure
- PVC loss
- Node corruption
- Accidental deletion

## Recovery Sources

Vault backups:

~/backups/vault

GitOps manifests:

homelab-gitops

## Recovery Steps

1. Rebuild Ubuntu
2. Reinstall K3s
3. Reinstall ArgoCD
4. Restore GitOps repository
5. Deploy Vault
6. Restore /vault/data
7. Start Vault
8. Unseal Vault
9. Verify External Secrets

## Validation

kubectl get clustersecretstore

kubectl get externalsecret -A

kubectl get secret -A
