# Disaster Recovery Runbook

## Document Information

| Attribute      | Value                     |
| -------------- | ------------------------- |
| Document Name  | Disaster Recovery Runbook |
| Platform       | NEXO-RAIDEN               |
| Classification | Runbook                   |
| Criticality    | Tier 1                    |
| Owner          | Platform Engineering      |

---

# 1. Executive Summary

This document defines the disaster recovery strategy for the NEXO-RAIDEN platform.

The objective is to restore platform services following catastrophic failures.

Examples:

* Raspberry Pi failure
* SD card corruption
* Kubernetes failure
* Data loss
* Vault loss
* PostgreSQL corruption

---

# 2. Recovery Philosophy

NEXO-RAIDEN recovery is based on:

GitOps
+
Backups
+
Documentation

Infrastructure is rebuilt from Git.

Data is restored from backups.

---

# 3. Recovery Objectives

## Recovery Time Objective (RTO)

Target:

4 Hours

---

## Recovery Point Objective (RPO)

Target:

24 Hours

---

# 4. Critical Components

Tier 1 Services:

* Ubuntu Server
* K3s
* ArgoCD
* Vault
* PostgreSQL
* Keycloak

---

# 5. Disaster Classification

## Level 1

Single Application Failure

Examples:

* Keycloak Pod Crash
* PostgreSQL Pod Restart

Recovery:

Application Level

---

## Level 2

Platform Service Failure

Examples:

* Vault Failure
* PostgreSQL Corruption

Recovery:

Component Recovery

---

## Level 3

Cluster Failure

Examples:

* K3s Corruption
* Kubernetes Unavailable

Recovery:

Cluster Recovery

---

## Level 4

Infrastructure Failure

Examples:

* Raspberry Pi Failure
* Storage Failure

Recovery:

Full Platform Recovery

---

# 6. Full Platform Recovery Sequence

Recovery Order:

1. Ubuntu Server
2. K3s
3. ArgoCD
4. Vault
5. External Secrets
6. PostgreSQL
7. Keycloak
8. Observability
9. Applications

Never restore out of order.

---

# 7. Infrastructure Recovery

## Raspberry Pi Replacement

Install:

Ubuntu Server 24.04 LTS

---

## Host Configuration

Configure:

Hostname:

nexo-raiden-lab-01

User:

nexo-raiden

---

## Verify Connectivity

```bash
ping github.com
```

---

# 8. Kubernetes Recovery

Install:

K3s

Verify:

```bash
kubectl get nodes
```

Expected:

```text
Ready
```

---

# 9. ArgoCD Recovery

Install ArgoCD.

Connect repository:

homelab-gitops

Verify:

```bash
kubectl get applications -n argocd
```

Expected:

All applications synchronized.

---

# 10. Vault Recovery

## Restore PVC

Restore:

Vault Storage Backup

---

## Verify

```bash
kubectl get pods -n security
```

---

## Validate

Confirm:

* Secrets Available
* External Secrets Synchronizing

---

# 11. PostgreSQL Recovery

## Restore Database

Use latest backup:

```bash
~/backups/postgresql
```

---

## Verify

Login:

```bash
psql
```

Validate:

* Databases
* Users
* Roles

---

# 12. Keycloak Recovery

Verify:

```bash
kubectl get pods -n platform
```

---

Validate:

* Login Works
* TLS Works
* Realms Available

---

# 13. Certificate Recovery

Verify:

```bash
kubectl get certificates -A
```

Expected:

```text
READY=True
```

---

# 14. Secret Recovery

Verify:

```bash
kubectl get externalsecrets -A
```

Expected:

```text
Ready=True
```

---

# 15. Validation Checklist

Infrastructure:

□ Node Ready

Kubernetes:

□ Pods Healthy

GitOps:

□ Applications Synced

Security:

□ Vault Healthy

Data:

□ PostgreSQL Healthy

Identity:

□ Keycloak Healthy

Certificates:

□ TLS Valid

---

# 16. Post-Recovery Activities

Perform:

* Health Checks
* Backup Validation
* Documentation Review
* Incident Report

---

# 17. Recovery Testing

Frequency:

Quarterly

Test:

* Vault Restore
* PostgreSQL Restore
* Cluster Recovery

Document results.

---

# 18. Future Improvements

Planned:

* SSD Storage
* Automated Restore Testing
* Offsite Backups
* S3 Replication
* Multi-Node Cluster

---

# 19. Related Documentation

platform-topology.md

platform-operations.md

platform-slos.md

vault-recovery.md

postgresql-recovery.md

cluster-recovery.md

---

# 20. Summary

The NEXO-RAIDEN disaster recovery process relies on GitOps, backups, and repeatable operational procedures.

Recovery prioritizes restoration of infrastructure, identity, secrets, and data before application services.

Following this runbook ensures the platform can be rebuilt consistently after catastrophic failure.
