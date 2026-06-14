# Ingress NGINX Architecture

## Current Architecture

Browser
↓
Router
↓
Raspberry Pi
↓
K3s
↓
Ingress NGINX
↓
Application

---

# Request Flow

Example:

https://keycloak.nexo-raiden.local

↓

Ingress Controller

↓

Keycloak Service

↓

Keycloak Pod

---

# TLS Flow

Browser
↓ HTTPS
Ingress NGINX
↓ HTTP
Keycloak

TLS terminates at Ingress.

---

# Kubernetes Resources

IngressClass

Name:

nginx

Purpose:

Default routing engine.

---

# Services

Ingress NGINX Controller Service

Exposes:

- HTTP
- HTTPS

---

# Dependencies

cert-manager

Provides certificates.

Keycloak

Consumes ingress routes.

Future:

Grafana
ArgoCD
RabbitMQ UI
AI Services
