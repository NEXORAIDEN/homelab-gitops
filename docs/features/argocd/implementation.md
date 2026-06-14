# ArgoCD Implementation

## Files

### apps/platform-root.yaml

Purpose:

Bootstrap the entire platform.

Creates:

- Root Application

---

### apps/argocd-projects.yaml

Purpose:

Deploy AppProjects.

---

### clusters/lab-01/argocd-projects/nexo-raiden-platform-project.yaml

Purpose:

Defines:

- Allowed repositories
- Allowed namespaces
- Cluster permissions

Current Allowed Repositories:

- GitHub
- Ingress NGINX
- HashiCorp
- External Secrets
- Jetstack

---

# Installation Verification

kubectl get applications -n argocd

Expected:

All applications Synced and Healthy.
