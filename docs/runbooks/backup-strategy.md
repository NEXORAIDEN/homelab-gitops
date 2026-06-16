# Backup Strategy

## Purpose

This runbook defines the backup strategy for the NEXO-RAIDEN platform.

## Critical Persistent Volumes

| Namespace | PVC | Size | Purpose |
|---|---|---|---|
| data | nexo-raiden-postgresql-pvc | 10Gi | PostgreSQL data |
| security | data-vault-0 | 5Gi | Vault data |

## Risk

Both volumes currently use `local-path` storage with reclaim policy `Delete`.

If a PVC is deleted, the backing volume may also be deleted.

## Backup Priority

1. Git repository
2. Vault data
3. PostgreSQL data
4. Keycloak configuration via PostgreSQL
5. TLS certificates if needed

## PostgreSQL Backup Method

Use logical backups with `pg_dump`.

## Vault Backup Method

Current Vault uses file storage.

Short-term backup method:

- backup Vault PVC data
- document unseal procedure
- keep unseal keys outside Git

Future backup method:

- Vault raft snapshot or HCP Vault
- encrypted off-node backup

## Recovery Order

1. Restore Git repository
2. Restore K3s and ArgoCD
3. Restore Vault
4. Unseal Vault
5. Restore PostgreSQL
6. Restore Keycloak
7. Verify applications
