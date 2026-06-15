# NEXO-RAIDEN Platform Topology

## Document Information

| Attribute               | Value                                                                     |
| ----------------------- | ------------------------------------------------------------------------- |
| Document Name           | Platform Topology                                                         |
| Document Type           | Master Architecture Reference                                             |
| Platform                | NEXO-RAIDEN                                                               |
| Environment             | Raspberry Pi 5 Homelab                                                    |
| Kubernetes Distribution | K3s                                                                       |
| Operating System        | Ubuntu Server 24.04 LTS                                                   |
| GitOps Engine           | ArgoCD                                                                    |
| Repository              | homelab-gitops                                                            |
| Owner                   | NEXO-RAIDEN                                                               |
| Audience                | Platform Engineers, Cloud Engineers, DevOps Engineers, Security Engineers |
| Purpose                 | Complete Platform Architecture Reference                                  |

---

# 1. Executive Summary

NEXO-RAIDEN is a cloud-native Platform Engineering environment designed to provide a production-grade learning, experimentation, portfolio, and migration platform.

The platform is intentionally designed around modern engineering principles:

* GitOps
* Infrastructure as Code
* Platform Engineering
* Cloud Native Architecture
* DevSecOps
* Identity Management
* Secret Management
* Observability
* Event-Driven Systems
* AI Engineering

The current implementation runs on a Raspberry Pi 5 using Ubuntu Server 24.04 and K3s Kubernetes.

The architecture is intentionally structured to evolve through multiple maturity phases:

```text
Phase 1
Homelab Foundation

↓

Phase 2
Production-Grade Platform

↓

Phase 3
AWS Cloud Migration

↓

Phase 4
Multi-Cloud Platform

↓

Phase 5
AI Platform Engineering
```

The goal is not merely to run applications but to build a complete platform that demonstrates enterprise-grade engineering practices.

---

# 2. Platform Vision

The NEXO-RAIDEN platform exists to demonstrate how a modern cloud-native platform should be designed, secured, operated, monitored, and evolved.

The platform should provide:

* Centralized identity management
* Centralized secret management
* Secure ingress
* Automated certificate management
* Declarative infrastructure
* GitOps operations
* Persistent data services
* Event-driven communication
* Full observability
* AI service hosting

The platform must remain:

* Portable
* Reproducible
* Auditable
* Secure
* Extensible

Every component must support future migration into AWS without major redesign.

---

# 3. Platform Objectives

## Engineering Objectives

The platform must provide hands-on experience with:

### Platform Engineering

* Kubernetes
* GitOps
* Infrastructure Automation
* Cluster Operations

### Cloud Engineering

* AWS
* Terraform
* Networking
* Security

### Backend Engineering

* Java 21
* Spring Boot 3
* PostgreSQL
* RabbitMQ
* Kafka

### DevSecOps

* Vault
* External Secrets
* cert-manager
* OPA
* Trivy
* Falco
* Cosign

### AI Engineering

* Ollama
* Qdrant
* RAG
* LLM Workflows

---

# 4. Architecture Principles

The platform follows strict engineering principles.

## GitOps First

Every infrastructure change must originate from Git.

Direct changes to Kubernetes resources are discouraged and considered configuration drift.

---

## Documentation First

Every feature must include:

* README
* Architecture documentation
* Operational documentation
* Troubleshooting documentation
* Recovery documentation

Documentation is considered a deliverable.

---

## Secrets Never In Git

Secrets must never be stored:

* In manifests
* In Helm values
* In repositories

Vault is the source of truth.

---

## TLS Everywhere

Every externally exposed application must use HTTPS.

No production-facing service should operate without TLS.

---

## Cloud Portability

Every service deployed locally must have an AWS equivalent.

Examples:

| Homelab         | AWS     |
| --------------- | ------- |
| K3s             | EKS     |
| PostgreSQL Pod  | RDS     |
| NodePort        | ALB     |
| Self-Signed TLS | ACM     |
| Local DNS       | Route53 |

---

# 5. Current Platform Status

## Foundation Layer

| Component         | Status      |
| ----------------- | ----------- |
| Ubuntu Server     | Operational |
| K3s               | Operational |
| GitHub Repository | Operational |
| ArgoCD            | Operational |

## Security Layer

| Component        | Status      |
| ---------------- | ----------- |
| Vault            | Operational |
| External Secrets | Operational |
| cert-manager     | Operational |
| TLS Certificates | Operational |

## Identity Layer

| Component          | Status      |
| ------------------ | ----------- |
| Keycloak           | Operational |
| PostgreSQL Backend | Operational |

## Data Layer

| Component         | Status      |
| ----------------- | ----------- |
| PostgreSQL        | Operational |
| Persistent Volume | Operational |

## Monitoring Layer

| Component     | Status      |
| ------------- | ----------- |
| Prometheus    | Operational |
| Grafana       | Operational |
| Node Exporter | Operational |

---

# 6. Physical Infrastructure Layer

## Hardware Overview

Current infrastructure consists of:

```text
Raspberry Pi 5
8 GB RAM
ARM64
512 GB Storage
```

Hostname:

```text
nexo-raiden-lab-01
```

Purpose:

Provide a low-cost cloud-native platform engineering environment.

---

## Physical Network

```text
MacBook
    │
    ▼
Home Network
    │
    ▼
Router
    │
    ▼
Raspberry Pi
    │
    ▼
Ubuntu Server
```

The Raspberry Pi serves as the physical host for all platform services.

---

# 7. Operating System Layer

## Operating System

```text
Ubuntu Server 24.04 LTS
```

Purpose:

Provide a stable and supported Linux foundation for Kubernetes.

---

## Host Responsibilities

The operating system provides:

* Process management
* Storage management
* Networking
* Security updates
* Kubernetes runtime support

---

## Host-Level Services

Current host services:

* SSH
* Container Runtime
* K3s
* Node Exporter

Future host services:

* Auditd
* Falco
* OSQuery

---

# 8. Kubernetes Layer

## Cluster Overview

Current cluster:

```text
K3s
Single Node
ARM64
```

Purpose:

Provide orchestration for all platform workloads.

---

## Kubernetes Responsibilities

The cluster provides:

* Scheduling
* Service Discovery
* Persistent Storage
* Networking
* Resource Management
* Secret Distribution

---

## Kubernetes Design Principles

Applications must:

* Be declarative
* Be reproducible
* Be GitOps-managed
* Use namespace isolation
* Consume secrets through External Secrets

---

# 9. Namespace Topology

Current namespaces:

```text
argocd
security
platform
data
monitoring
applications
ai
```

## argocd

Purpose:

GitOps control plane.

Contains:

* ArgoCD Server
* Repo Server
* Application Controller
* Redis

## security

Purpose:

Platform security services.

Contains:

* Vault
* External Secrets
* cert-manager

## platform

Purpose:

Shared platform services.

Contains:

* Ingress NGINX
* Keycloak

## data

Purpose:

Persistent data services.

Contains:

* PostgreSQL

## monitoring

Purpose:

Observability stack.

Contains:

* Prometheus
* Grafana
* Node Exporter

## ai

Purpose:

Future AI workloads.

Contains:

* Ollama
* Qdrant
* RAG Services

---

# 10. Repository Architecture

Repository:

```text
homelab-gitops
```

Current structure:

```text
apps/
clusters/
infrastructure/
docs/
```

Purpose:

Provide separation between:

* Deployments
* Cluster Configuration
* Infrastructure Resources
* Documentation

```
```
