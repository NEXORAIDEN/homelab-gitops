# Keycloak TLS Ingress# Keycloak TLS Architecture

## Overview

This document describes the TLS implementation for Keycloak within the NEXO-RAIDEN Platform.

The objective is to expose Keycloak securely through NGINX Ingress using certificates managed by cert-manager.

The implementation provides:

* Encrypted communication
* Centralized ingress management
* Automated certificate lifecycle
* Cloud migration readiness

---

# Business Problem

Without TLS:

```text
Browser
    ↓
HTTP
    ↓
Keycloak
```

Problems:

* Credentials transmitted unencrypted
* Browser security warnings
* No production readiness
* Not compliant with security best practices

---

# Solution Architecture

```text
┌──────────────────────────┐
│ Browser                  │
└─────────────┬────────────┘
              │ HTTPS
              ▼
┌──────────────────────────┐
│ NGINX Ingress Controller │
└─────────────┬────────────┘
              │ HTTP
              ▼
┌──────────────────────────┐
│ Keycloak Service         │
└─────────────┬────────────┘
              ▼
┌──────────────────────────┐
│ Keycloak Pod             │
└──────────────────────────┘
```

TLS is terminated at the ingress layer.

Keycloak receives internal HTTP traffic.

---

# Components

## NGINX Ingress

Purpose:

Provides centralized entry point into the cluster.

Responsibilities:

* Host routing
* TLS termination
* Traffic forwarding

Namespace:

```text
platform
```

Verification:

```bash
kubectl get ingress -A
```

---

## cert-manager

Purpose:

Automates certificate creation and renewal.

Namespace:

```text
security
```

Resources Created:

```text
Certificate
CertificateRequest
Secret
```

Verification:

```bash
kubectl get certificate -A
```

---

## ClusterIssuer

Purpose:

Defines how certificates are issued.

Current Type:

```text
SelfSigned
```

Future Type:

```text
Let's Encrypt
```

Verification:

```bash
kubectl get clusterissuer
```

---

# Files

## apps/cert-manager.yaml

Purpose:

Deploys cert-manager using ArgoCD.

Flow:

Git
↓
ArgoCD
↓
Helm Chart
↓
cert-manager

---

## apps/cert-manager-issuers.yaml

Purpose:

Deploys ClusterIssuer resources.

Flow:

Git
↓
ArgoCD
↓
ClusterIssuer

---

## infrastructure/security/cert-manager/cluster-issuer.yaml

Purpose:

Defines certificate authority.

Current:

```yaml
selfSigned: {}
```

---

## infrastructure/platform/keycloak/ingress.yaml

Purpose:

Creates HTTPS endpoint.

Important Sections:

### ClusterIssuer Annotation

```yaml
cert-manager.io/cluster-issuer:
  nexo-raiden-selfsigned
```

Tells cert-manager how to issue certificates.

### TLS Block

```yaml
tls:
```

Requests certificate creation.

### Rules

```yaml
rules:
```

Routes incoming traffic.

---

## infrastructure/platform/keycloak/deployment.yaml

Purpose:

Configures Keycloak proxy awareness.

Important Variables:

### KC_HOSTNAME

Defines public URL.

### KC_PROXY_HEADERS

Allows Keycloak to trust ingress forwarding headers.

### KC_HTTP_ENABLED

Allows HTTP traffic behind TLS ingress.

---

# Deployment Flow

```text
Git Push
    ↓
ArgoCD Detects Change
    ↓
Ingress Created
    ↓
cert-manager Sees TLS Request
    ↓
Certificate Generated
    ↓
TLS Secret Created
    ↓
NGINX Uses Secret
    ↓
HTTPS Available
```

---

# Verification Procedures

## Verify Ingress

```bash
kubectl get ingress -n platform
```

Expected:

```text
PORTS 80,443
```

---

## Verify Certificate

```bash
kubectl get certificate -n platform
```

Expected:

```text
READY=True
```

---

## Verify Secret

```bash
kubectl get secret keycloak-nexo-raiden-local-tls -n platform
```

Expected:

```text
kubernetes.io/tls
```

---

## Verify Browser Access

URL:

```text
https://keycloak.nexo-raiden.local:30443
```

Expected:

Keycloak Administration Console.

---

# Troubleshooting

## 400 Bad Request

Cause:

Browser sends HTTP to HTTPS endpoint.

Resolution:

Use explicit:

```text
https://
```

---

## Certificate Missing

Verify:

```bash
kubectl get certificate -A
kubectl get clusterissuer
```

---

## Ingress Missing TLS

Verify:

```bash
kubectl describe ingress
```

Check:

```yaml
tls:
```

section exists.

---

# AWS Migration

Current:

```text
cert-manager
SelfSigned
```

Future:

```text
Route53
Let's Encrypt
AWS ACM
```

Result:

Public trusted certificates.

---

# Current Status

```text
Keycloak HTTPS     Working
Certificate        Ready
Ingress TLS        Working
Browser Access     Working
```


## Purpose

Expose Keycloak securely over HTTPS through Ingress NGINX.

## URL

```text
https://keycloak.nexo-raiden.local:30443
