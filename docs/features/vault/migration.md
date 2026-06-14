# Vault AWS Migration

Current

Raspberry Pi
↓
Vault
↓
File Storage

Future

AWS
↓
HCP Vault
or
Vault HA Cluster

---

# Migration Options

Option 1

HCP Vault

Recommended

Benefits:

- Managed service
- High availability
- Backups
- Monitoring

---

Option 2

Self-Hosted Vault

AWS EKS

Benefits:

- Full control

Disadvantages:

- Operational overhead

---

# AWS Service Mapping

| Current | Future |
|----------|---------|
| Vault | HCP Vault |
| File Storage | Managed Storage |
| Root Token | IAM Auth |
| Local Secrets | AWS Integrated Secrets |

---

# Future Enhancements

- Dynamic Database Credentials
- Secret Rotation
- Vault Policies
- Kubernetes Authentication
- AWS IAM Authentication
