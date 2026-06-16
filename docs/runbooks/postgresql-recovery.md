# PostgreSQL Recovery

## Purpose

Restore PostgreSQL after:

- Node failure
- SD card failure
- PVC loss
- Database corruption

## Available Backups

Stored in:

~/backups/postgresql

## Verify Backup

gunzip -c backup.sql.gz | head

## Restore

kubectl exec -i -n data deploy/nexo-raiden-postgresql -- \
  psql -U nexo_admin < backup.sql

## Validation

Verify:

- Keycloak login
- Database connectivity
- Application startup

## Recovery Order

1. Restore PVC
2. Restore PostgreSQL
3. Verify Keycloak
4. Verify Applications

## Database User

The PostgreSQL administrative user is:

nexo_admin

The platform does not use the default postgres role.

Credentials are managed through:

Vault
↓
External Secrets
↓
Kubernetes Secret
↓
PostgreSQL
