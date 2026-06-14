# NEXO-RAIDEN Platform Overview

## Purpose

The NEXO-RAIDEN Platform is a self-hosted Platform Engineering, DevSecOps, Cloud, Security and AI laboratory built on a Raspberry Pi 5 running Ubuntu Server 24.04 LTS.

The platform serves four primary goals:

1. Learning and certification preparation
2. Portfolio development
3. Client demonstration environment
4. Foundation for future AWS and multi-cloud deployments

---

# Platform Objectives

The platform demonstrates:

* GitOps
* Kubernetes
* Infrastructure as Code
* DevSecOps
* Identity and Access Management
* Secret Management
* Observability
* Event-Driven Architecture
* AI Infrastructure

---

# Physical Architecture

```text
MacBook Pro
    │
    ▼
Home Router
    │
    ▼
Raspberry Pi 5
(8 GB RAM)
    │
    ▼
Ubuntu Server 24.04
```

---

# Platform Architecture

```text
Ubuntu Server
    │
    ▼
K3s Kubernetes
    │
    ▼
ArgoCD
    │
    ▼
Platform Services
```

---

# Current Technology Stack

## Operating System

Ubuntu Server 24.04 LTS

Purpose:

Provides the base operating system for the platform.

---

## Kubernetes

K3s

Purpose:

Lightweight Kubernetes distribution optimized for ARM hardware.

Responsibilities:

* Container orchestration
* Scheduling
* Networking
* Service discovery
* Storage management

---

## GitOps

ArgoCD

Purpose:

Declarative application deployment.

Responsibilities:

* Continuous reconciliation
* Drift detection
* Automated deployment
* Self-healing

---

# Security Layer

## Keycloak

Purpose:

Identity and Access Management

Capabilities:

* Authentication
* Authorization
* OAuth2
* OpenID Connect
* Single Sign-On

---

## Vault

Purpose:

Centralized secret management

Capabilities:

* Secret storage
* Secret versioning
* Secret auditing
* Secret rotation

---

## External Secrets

Purpose:

Synchronize Vault secrets into Kubernetes

Capabilities:

* Secret synchronization
* Secret lifecycle management
* Secret abstraction

---

## cert-manager

Purpose:

Certificate lifecycle management

Capabilities:

* Certificate issuance
* Certificate renewal
* TLS automation

---

# Data Layer

## PostgreSQL

Purpose:

Primary relational database

Current Consumers:

* Keycloak
* Future Spring Boot services

---

# Networking Layer

## NGINX Ingress Controller

Purpose:

Central platform entry point

Responsibilities:

* Routing
* TLS termination
* Host-based access
* Reverse proxy

---

# Current Platform Services

| Service          | Status      |
| ---------------- | ----------- |
| K3s              | Operational |
| ArgoCD           | Operational |
| PostgreSQL       | Operational |
| Keycloak         | Operational |
| NGINX Ingress    | Operational |
| Vault            | Operational |
| External Secrets | Operational |
| cert-manager     | Operational |

---

# Secret Management Flow

```text
Vault
   │
   ▼
External Secrets
   │
   ▼
Kubernetes Secrets
   │
   ▼
Applications
```

Current Applications:

* Keycloak
* PostgreSQL

---

# HTTPS Flow

```text
Browser
    │
 HTTPS
    ▼
NGINX Ingress
    │
 HTTP
    ▼
Application
```

TLS Termination:

NGINX Ingress Controller

Certificate Management:

cert-manager

---

# Future Components

## Observability

Planned:

* Prometheus
* Grafana
* Alertmanager
* ELK Stack

---

## Messaging

Planned:

* RabbitMQ
* Kafka

---

## Platform Services

Planned:

* Spring Boot Services
* OpenAPI
* Flyway
* Testcontainers

---

## AI Platform

Planned:

* Ollama
* Qdrant
* RAG Services

---

# AWS Migration Path

Current:

```text
Raspberry Pi 5
Ubuntu
K3s
Vault
PostgreSQL
```

Target:

```text
AWS
EKS
HCP Vault
RDS PostgreSQL
Route53
ALB
ACM
```

---

# Documentation Structure

Each platform component contains:

* README
* Architecture
* Implementation
* Operations
* Troubleshooting
* Migration

This ensures every component is fully documented from business requirement through operational support.

---

# Current Platform Status

Platform Phase:

Foundation Complete

Major Components:

Operational

Security Foundation:

Operational

Secret Management:

Operational

TLS Infrastructure:

Operational

Next Milestone:

Observability Platform
(Prometheus + Grafana)
