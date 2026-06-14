# ArgoCD

## Purpose

ArgoCD is the GitOps engine of the NEXO-RAIDEN platform.

It continuously synchronizes Kubernetes resources with the desired state stored in Git.

Without ArgoCD:

- Manual deployments
- Configuration drift
- Inconsistent environments

With ArgoCD:

- Declarative deployments
- Automated synchronization
- Self-healing infrastructure
- Auditability

---

# Business Value

ArgoCD provides:

- Deployment automation
- Configuration consistency
- Faster recovery
- Infrastructure traceability
- Reduced operational risk

---

# Current Status

| Component | Status |
|------------|---------|
| ArgoCD Server | Operational |
| AppProject | Operational |
| Root Application | Operational |
| Auto Sync | Enabled |
| Self Heal | Enabled |

---

# Dependencies

- K3s
- GitHub Repository
- Ingress NGINX (future HTTPS access)

---

# Related Components

- Vault
- PostgreSQL
- Keycloak
- cert-manager
- External Secrets

All are deployed through ArgoCD.
