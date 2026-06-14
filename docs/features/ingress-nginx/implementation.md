# Ingress NGINX Implementation

## ArgoCD Application

File:

apps/ingress-nginx.yaml

Purpose:

Deploys Ingress NGINX Helm chart.

---

# Helm Repository

https://kubernetes.github.io/ingress-nginx

---

# Helm Chart

Chart:

ingress-nginx

Version:

4.11.3

---

# Namespace

platform

---

# Created Resources

Deployment

ingress-nginx-controller

---

IngressClass

nginx

---

Service

ingress-nginx-controller

---

# Verification

kubectl get ingressclass

Expected:

nginx

---

kubectl get pods -n platform

Expected:

ingress-nginx-controller Running
