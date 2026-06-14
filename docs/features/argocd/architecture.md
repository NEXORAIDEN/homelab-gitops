# ArgoCD Architecture

## GitOps Flow

Git Repository
↓
ArgoCD
↓
Kubernetes API
↓
Cluster Resources

---

# Current Repository Structure

homelab-gitops/

├── apps/
├── clusters/
├── infrastructure/
├── docs/

---

# Deployment Flow

Developer
↓
git push
↓
GitHub
↓
ArgoCD detects change
↓
Sync
↓
Kubernetes updated

---

# Application Hierarchy

nexo-raiden-platform-root
↓
nexo-raiden-argocd-projects
↓
Platform Applications

Examples:

- PostgreSQL
- Keycloak
- Vault
- External Secrets
- cert-manager

---

# Security Model

ArgoCD manages:

- Namespaces
- Deployments
- Services
- Ingresses
- Secrets (via External Secrets)

ArgoCD does not store secrets directly.

Secrets are managed through:

Vault
↓
External Secrets
↓
Kubernetes Secret
