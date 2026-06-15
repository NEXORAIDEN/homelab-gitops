# NEXO-RAIDEN Networking Architecture

## Document Information

| Attribute     | Value                  |
| ------------- | ---------------------- |
| Document Type | Architecture Reference |
| Domain        | Networking             |
| Platform      | NEXO-RAIDEN            |
| Kubernetes    | K3s                    |
| Environment   | Raspberry Pi 5         |
| OS            | Ubuntu Server 24.04    |

---

# 1. Purpose

This document describes the networking architecture of the NEXO-RAIDEN platform.

It explains:

* External access
* Internal Kubernetes networking
* DNS resolution
* Service discovery
* Ingress routing
* TLS termination
* Future AWS networking design

---

# 2. Networking Objectives

The networking design aims to provide:

* Secure application access
* Centralized ingress
* Internal service isolation
* Cloud portability
* Scalable service discovery
* TLS support

---

# 3. Physical Network Architecture

Current Environment

MacBook
↓
Home Router
↓
Raspberry Pi 5
↓
Ubuntu Server 24.04
↓
K3s

Purpose:

Provides the physical foundation of the platform.

---

# 4. Logical Platform Architecture

Browser
↓
DNS Resolution
↓
Ingress NGINX
↓
Kubernetes Service
↓
Pod

This pattern is used by all applications.

---

# 5. Current External Access Model

Current Entry Points

| Port  | Purpose |
| ----- | ------- |
| 22    | SSH     |
| 30080 | HTTP    |
| 30443 | HTTPS   |

Example:

https://keycloak.nexo-raiden.local:30443

↓

Ingress NGINX

↓

Keycloak Service

↓

Keycloak Pod

---

# 6. DNS Architecture

Current Local DNS Model

Hosts File

Example:

192.168.178.200 keycloak.nexo-raiden.local

Purpose:

Local name resolution.

---

# Future DNS Architecture

Route53

↓

Public DNS

↓

Ingress

Benefits:

* No hosts file
* Automatic certificate validation
* Internet accessibility

---

# 7. Kubernetes Networking Model

K3s uses an internal pod network.

Every pod receives:

* Internal IP
* Internal DNS
* Service discovery

Example:

10.42.x.x

Pods communicate internally.

---

# 8. Service Discovery

Applications never communicate using pod IPs.

Applications communicate through services.

Example:

nexo-raiden-postgresql.data.svc.cluster.local

Benefits:

* Stable endpoint
* Pod replacement transparency
* Scalability

---

# 9. Kubernetes Service Types

## ClusterIP

Internal only.

Examples:

* PostgreSQL
* Vault
* Keycloak Service

Use Case:

Application-to-application communication.

---

## NodePort

Current ingress exposure.

Examples:

30080
30443

Use Case:

External access into homelab.

---

## LoadBalancer

Future AWS implementation.

Use Case:

Internet-facing services.

---

# 10. Ingress Architecture

Current Ingress Controller:

NGINX

Namespace:

platform

Purpose:

Single platform entry point.

---

# 11. Request Flow

Browser
↓
Ingress NGINX
↓
Service
↓
Pod

Example:

keycloak.nexo-raiden.local

↓

Ingress

↓

nexo-raiden-keycloak Service

↓

Keycloak Pod

---

# 12. TLS Architecture

Browser
↓ HTTPS
Ingress NGINX
↓ HTTP
Application

TLS Termination Location:

Ingress NGINX

Benefits:

* Simplified certificates
* Central management
* Reduced application complexity

---

# 13. Certificate Flow

Ingress
↓
cert-manager
↓
ClusterIssuer
↓
Certificate
↓
TLS Secret
↓
Ingress HTTPS

Current Certificate:

keycloak-nexo-raiden-local-tls

---

# 14. Current Routes

| Host                       | Backend  |
| -------------------------- | -------- |
| keycloak.nexo-raiden.local | Keycloak |

Future:

| Host                      | Backend     |
| ------------------------- | ----------- |
| argocd.nexo-raiden.local  | ArgoCD      |
| grafana.nexo-raiden.local | Grafana     |
| api.nexo-raiden.local     | Spring APIs |
| ai.nexo-raiden.local      | AI Platform |

---

# 15. Network Security Model

Publicly Accessible

* HTTPS Ingress
* SSH

Internal Only

* PostgreSQL
* Vault
* External Secrets
* cert-manager

Security Principle:

Applications should never expose internal services directly.

---

# 16. Namespace Communication

security

↓

platform

↓

data

Current:

Open communication

Future:

NetworkPolicies

Benefits:

* Restrict lateral movement
* Reduce attack surface
* Enforce least privilege

---

# 17. Current Networking Components

| Component         | Purpose             |
| ----------------- | ------------------- |
| Ingress NGINX     | Routing             |
| Services          | Service discovery   |
| CoreDNS           | DNS                 |
| cert-manager      | TLS                 |
| Ingress Resources | Application routing |

---

# 18. Operational Verification

Verify Ingress:

kubectl get ingress -A

---

Verify Services:

kubectl get svc -A

---

Verify Endpoints:

kubectl get endpoints -A

---

Verify DNS:

kubectl get svc -n kube-system

---

Verify TLS:

kubectl get certificate -A

---

Verify Keycloak Route:

curl -k -I 
https://keycloak.nexo-raiden.local:30443

---

# 19. Known Limitations

Current:

* Hosts file DNS
* Self-signed certificates
* NodePort exposure
* No NetworkPolicies

---

# 20. Networking Roadmap

Phase 1

✓ Ingress NGINX
✓ DNS
✓ TLS

Phase 2

□ ArgoCD HTTPS
□ Grafana HTTPS

Phase 3

□ NetworkPolicies
□ Internal Service Segmentation

Phase 4

□ Route53
□ AWS Load Balancer Controller
□ ALB

---

# 21. AWS Networking Mapping

| Current         | AWS               |
| --------------- | ----------------- |
| NodePort        | ALB               |
| Hosts File      | Route53           |
| K3s DNS         | Route53 + CoreDNS |
| Self-Signed TLS | ACM               |
| Raspberry Pi    | EKS               |

---

# 22. Networking Architecture Summary

The NEXO-RAIDEN networking architecture provides:

* Centralized ingress
* Internal service discovery
* TLS termination
* DNS resolution
* Cloud portability

and serves as the foundation for all future platform services.
