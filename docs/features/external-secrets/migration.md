# External Secrets AWS Migration

Current

Vault
↓
External Secrets
↓
Kubernetes Secret

---

Future Option 1

AWS Secrets Manager
↓
External Secrets
↓
Kubernetes Secret

---

Future Option 2

HCP Vault
↓
External Secrets
↓
Kubernetes Secret

---

# Migration Benefits

No application changes required.

Applications continue using:

Kubernetes Secrets

---

# AWS Mapping

| Current | Future |
|----------|---------|
| Vault | HCP Vault |
| Vault | AWS Secrets Manager |
| Local K3s | EKS |

---

# Future Enhancements

- Dynamic Credentials
- Automatic Rotation
- Secret Auditing
- Multi-cluster Secret Distribution
