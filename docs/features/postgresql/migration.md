# PostgreSQL AWS Migration

Current

K3s
↓
PostgreSQL Container
↓
PVC

Future

EKS
↓
Amazon RDS PostgreSQL

---

# Migration Benefits

- Automated backups
- Multi-AZ
- High Availability
- Read Replicas
- Automated patching

---

# Migration Mapping

| Current | AWS |
|----------|------|
| PostgreSQL Pod | RDS PostgreSQL |
| PVC | EBS |
| Service | RDS Endpoint |
| Vault Secret | Secrets Manager |

---

# Future Enhancements

- Automated backups
- Point-in-time recovery
- High availability
- Database monitoring
