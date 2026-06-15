# NEXO-RAIDEN Data Architecture

## Document Information

| Attribute     | Value                  |
| ------------- | ---------------------- |
| Document Type | Architecture Reference |
| Domain        | Data Layer             |
| Platform      | NEXO-RAIDEN            |
| Database      | PostgreSQL             |
| Kubernetes    | K3s                    |
| Environment   | Raspberry Pi 5         |
| Namespace     | data                   |

---

# 1. Purpose

This document describes the data architecture of the NEXO-RAIDEN platform.

It explains:

* Data storage
* Database architecture
* Data consumers
* Data security
* Secret management
* Backup strategy
* Recovery procedures
* Future database evolution

---

# 2. Data Objectives

The platform data layer must provide:

* Persistent storage
* High reliability
* Secure credential management
* Backup capability
* Cloud portability
* Future scalability

---

# 3. Current Data Components

| Component             | Purpose                     |
| --------------------- | --------------------------- |
| PostgreSQL            | Primary relational database |
| PersistentVolumeClaim | Data persistence            |
| Vault                 | Credential storage          |
| External Secrets      | Credential synchronization  |
| Keycloak              | Current database consumer   |

---

# 4. Data Domain Architecture

Applications
│
▼
PostgreSQL Service
│
▼
PostgreSQL Pod
│
▼
Persistent Volume

Purpose:

Provide durable storage for platform workloads.

---

# 5. Current Database Architecture

Keycloak
│
▼
PostgreSQL Service
│
▼
PostgreSQL Database
│
▼
Persistent Storage

Current Consumer:

Keycloak

Future Consumers:

* Spring Boot Services
* Audit Services
* Event-Driven Services
* AI Services

---

# 6. Kubernetes Data Architecture

Namespace:

data

Resources:

Deployment

nexo-raiden-postgresql

Service

nexo-raiden-postgresql

ExternalSecret

nexo-raiden-postgresql-secret

PersistentVolumeClaim

postgresql-pvc

---

# 7. Database Connectivity

Applications never connect to Pods directly.

Applications connect through Services.

Current Endpoint:

nexo-raiden-postgresql.data.svc.cluster.local

Port:

5432

Benefits:

* Stable endpoint
* Pod replacement transparency
* Service discovery

---

# 8. Data Security Architecture

Database Security Layers

Application
↓
Kubernetes Secret
↓
External Secret
↓
Vault
↓
PostgreSQL

Purpose:

Prevent credential exposure.

---

# 9. Database Credential Flow

Vault

nexo/data/postgresql

↓

ClusterSecretStore

↓

ExternalSecret

↓

Kubernetes Secret

↓

PostgreSQL Deployment

Benefits:

* No hardcoded credentials
* Centralized secret management
* Future credential rotation

---

# 10. Current Database Secrets

Secret Name:

nexo-raiden-postgresql-secret

Contains:

POSTGRES_DB

POSTGRES_USER

POSTGRES_PASSWORD

Managed By:

External Secrets

Source:

Vault

---

# 11. Current Storage Model

PostgreSQL Container
↓
PersistentVolumeClaim
↓
Node Storage

Purpose:

Persist data across:

* Pod restarts
* Node reboots
* Deployment updates

---

# 12. Data Flow Example

Keycloak Login
↓
User Validation
↓
PostgreSQL Query
↓
User Record
↓
Authentication Result

Current Data Stored:

* Users
* Groups
* Roles
* Sessions
* Realms

---

# 13. Future Data Architecture

Spring Boot Services

Account Service
↓
PostgreSQL

Transaction Service
↓
PostgreSQL

Audit Service
↓
PostgreSQL

Purpose:

Central platform persistence layer.

---

# 14. Backup Strategy

Current State

Manual backups

Future State

Automated backups

Recommended Backup Targets:

* PostgreSQL Database
* Vault Data
* Git Repository

Priority:

Critical

---

# 15. Recovery Strategy

Recovery Order

1. Git Repository
2. Vault
3. PostgreSQL
4. Keycloak
5. Applications

Reason:

Applications depend on database availability.

---

# 16. Data Verification Procedures

Verify Deployment

kubectl get deployment -n data

---

Verify Pod

kubectl get pods -n data

---

Verify Service

kubectl get svc -n data

---

Verify PVC

kubectl get pvc -n data

---

Verify External Secret

kubectl get externalsecret -n data

---

Verify Secret

kubectl get secret -n data

---

# 17. Operational Commands

View Logs

kubectl logs 
-n data 
deploy/nexo-raiden-postgresql

---

Restart

kubectl rollout restart 
deployment/nexo-raiden-postgresql 
-n data

---

Connect

kubectl exec -it 
deploy/nexo-raiden-postgresql 
-n data -- bash

---

# 18. Known Risks

Current Limitations

* Single PostgreSQL instance
* No replication
* No automated backups
* No point-in-time recovery
* Single-node storage

---

# 19. Future Enhancements

Phase 1

✓ PostgreSQL
✓ Persistent Storage
✓ Vault Integration

Phase 2

□ Automated Backups
□ Backup Verification

Phase 3

□ PostgreSQL Monitoring
□ Query Performance Metrics

Phase 4

□ High Availability
□ Read Replicas

---

# 20. AWS Data Mapping

| Current        | AWS                   |
| -------------- | --------------------- |
| PostgreSQL Pod | Amazon RDS PostgreSQL |
| PVC            | EBS                   |
| Vault Secret   | Secrets Manager       |
| Local Storage  | Managed Storage       |

Benefits:

* Automated backups
* Multi-AZ
* Read replicas
* Managed operations

---

# 21. Data Architecture Summary

The NEXO-RAIDEN Data Layer provides:

* Persistent storage
* Secret-managed database credentials
* Service-based connectivity
* Kubernetes-native deployment
* Cloud portability

It serves as the foundational persistence layer for current and future platform services.
