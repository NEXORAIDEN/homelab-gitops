# Ingress NGINX

## Purpose

Ingress NGINX acts as the central entry point into the NEXO-RAIDEN Kubernetes platform.

It provides:

- HTTP Routing
- HTTPS Routing
- TLS Termination
- Reverse Proxy
- Host-based Routing
- Future Load Balancing

Without Ingress:

Applications must expose NodePorts directly.

With Ingress:

A single entry point handles all traffic.

---

# Business Value

Ingress NGINX provides:

- Centralized access management
- TLS termination
- Simplified DNS management
- Reduced operational complexity
- Cloud-native architecture

---

# Current Status

| Component | Status |
|------------|---------|
| Ingress Controller | Operational |
| IngressClass | Operational |
| Keycloak Route | Operational |
| TLS Support | Operational |
| cert-manager Integration | Operational |

---

# Current Routes

| Host | Service |
|---------|---------|
| keycloak.nexo-raiden.local | Keycloak |

Future:

| Host | Service |
|---------|---------|
| argocd.nexo-raiden.local | ArgoCD |
| grafana.nexo-raiden.local | Grafana |
| api.nexo-raiden.local | Spring APIs |
| ai.nexo-raiden.local | AI Platform |
