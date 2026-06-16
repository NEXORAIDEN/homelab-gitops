# Vault Backup

Vault currently uses file storage at `/vault/data`.

Backup method:

```bash
kubectl exec -n security vault-0 -- tar czf - /vault/data > ~/backups/vault/vault-file-storage-DATE.tar.gz

# Vault Backup Runbook

## Purpose

Vault stores platform secrets for NEXO-RAIDEN.

Current Vault storage backend:

```hcl
storage "file" {
  path = "/vault/data"
}


LATEST_VAULT_BACKUP=$(ls -t ~/backups/vault/vault-file-storage-*.tar.gz | head -1)
tar tzf "$LATEST_VAULT_BACKUP" | head -30
