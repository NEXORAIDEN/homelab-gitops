# Backup Automation

## Purpose

Automate daily backups for critical NEXO-RAIDEN stateful services.

## Scheduled Jobs

PostgreSQL:

```cron
0 2 * * * /home/nexo-raiden/scripts/postgresql-backup.sh >> /home/nexo-raiden/backups/postgresql/backup.log 2>&1


