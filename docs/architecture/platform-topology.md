# NEXO-RAIDEN Platform Topology

Version: 1.0
Classification: Internal Platform Architecture
Platform: NEXO-RAIDEN
Environment: Raspberry Pi 5 Homelab Platform
Primary Host: nexo-raiden-lab-01
Operating System: Ubuntu Server 24.04 LTS
Container Platform: K3s Kubernetes
GitOps Platform: ArgoCD
CI Platform: GitHub Actions
Author: NEXO-RAIDEN Platform Engineering

---

# 1. Document Information

## Purpose

This document serves as the authoritative architecture handbook for the NEXO-RAIDEN platform.

It describes:

* Platform architecture
* Infrastructure topology
* Security architecture
* Networking architecture
* Data architecture
* GitOps architecture
* CI/CD architecture
* Operational procedures
* Recovery architecture
* AWS migration strategy
* Future platform evolution

This document should be the first document read by new engineers and the final document reviewed before major platform releases.

---

# 2. Executive Summary

NEXO-RAIDEN is a cloud-native Platform Engineering, Security Engineering, and AI Engineering environment designed to demonstrate enterprise-grade architecture patterns using modern infrastructure technologies.

The platform currently operates on a Raspberry Pi 5 running Ubuntu Server and K3s Kubernetes and is designed to evolve toward AWS and multi-cloud deployment models.

The architecture is based on five core pillars:

1. Infrastructure Automation
2. GitOps Operations
3. Security by Design
4. Observability First
5. Cloud Portability

The platform demonstrates:

* Kubernetes Operations
* Platform Engineering
* DevSecOps
* Identity and Access Management
* Secret Management
* Secure Software Delivery
* Event-Driven Architecture
* AI Platform Engineering

---

# 3. Platform Vision

## Mission

Engineer Secure Intelligent Systems.

NEXO-RAIDEN provides a modern engineering platform that combines:

* Cloud Engineering
* Security Engineering
* Platform Engineering
* AI Engineering

into a single cohesive architecture.

---

## Strategic Objectives

The platform exists to:

* Demonstrate enterprise-grade engineering practices
* Provide a realistic production-like environment
* Enable continuous learning
* Support portfolio development
* Prepare for AWS and multi-cloud deployment
* Support future AI workloads

---

# 4. Platform Objectives

## Operational Objectives

* Infrastructure as Code
* GitOps Deployments
* Automated Recovery
* Secure Secret Management
* End-to-End Observability

---

## Security Objectives

* Zero hardcoded credentials
* Centralized secret management
* TLS everywhere
* Identity-first architecture
* Principle of least privilege

---

## Engineering Objectives

* Fully documented architecture
* Automated deployments
* Automated backups
* Disaster recovery procedures
* Repeatable platform builds

---

# 5. Architecture Principles

## Git as Source of Truth

All platform changes originate from Git.

Manual configuration drift is not permitted.

---

## Infrastructure as Code

Infrastructure must be declarative.

Examples:

* Kubernetes manifests
* ArgoCD Applications
* Terraform
* Ansible

---

## Security by Design

Security is integrated into every layer.

Examples:

* Vault
* External Secrets
* Keycloak
* TLS
* GitOps

---

## Documentation as a Deliverable

Documentation is a platform artifact.

A platform component is not considered complete until:

* Architecture documentation exists
* Operational documentation exists
* Recovery documentation exists

---

## Cloud Portability

Every platform decision should support future migration to:

* AWS
* Multi-cloud
* Hybrid-cloud

---

# 6. Platform Architecture Overview

## High-Level Architecture

Internet User
↓
Ingress NGINX
↓
TLS
↓
Keycloak
↓
Applications
↓
PostgreSQL

Platform Operations:

Engineer
↓
GitHub
↓
GitHub Actions
↓
ArgoCD
↓
Kubernetes

Secret Management:

Vault
↓
External Secrets
↓
Kubernetes Secrets
↓
Applications

Observability:

Applications
↓
Prometheus
↓
Grafana
↓
Engineers

---

# 7. Physical Infrastructure Layer

## Hardware

### Primary Platform Node

Component: Raspberry Pi 5

Responsibilities:

* Kubernetes Host
* Platform Services
* Development Environment
* Learning Environment

---

## Storage

Current Storage:

* Local SD Storage
* Kubernetes Persistent Volumes

Current PVCs:

### PostgreSQL

Namespace:

data

PVC:

nexo-raiden-postgresql-pvc

Capacity:

10Gi

---

### Vault

Namespace:

security

PVC:

data-vault-0

Capacity:

5Gi

---

## Risks

Current Risks:

* Single Node
* Single Storage Device
* Local Storage Only

Future Improvements:

* SSD Storage
* Offsite Backups
* Multi-Node Kubernetes

---

# 8. Operating System Layer

## Platform OS

Ubuntu Server 24.04 LTS

Responsibilities:

* Container Runtime Host
* Kubernetes Host
* Security Baseline
* Package Management

---

## Host Standards

Required:

* Automatic security updates
* SSH hardening
* Minimal package installation
* Backup automation

---

## Host Identity

Hostname:

nexo-raiden-lab-01

Primary User:

nexo-raiden

---

# 9. Kubernetes Layer

## Distribution

K3s

Purpose:

Lightweight Kubernetes distribution optimized for edge and homelab environments.

---

## Responsibilities

* Container orchestration
* Service discovery
* Storage management
* Networking
* Scheduling

---

## Current Cluster Model

Single Node

Current Mode:

Development and Platform Engineering Environment

Future Mode:

Production-Like Multi-Node Cluster

---

# 10. Namespace Topology

## Purpose

Namespaces provide logical isolation between platform domains.

---

## security

Contains:

* Vault
* External Secrets
* cert-manager

Purpose:

Platform security services.

---

## platform

Contains:

* Keycloak
* Ingress NGINX

Purpose:

Platform-facing services.

---

## data

Contains:

* PostgreSQL

Purpose:

Persistent data services.

---

## monitoring

Contains:

* Prometheus
* Grafana

Purpose:

Observability platform.

---

## ai

Reserved for:

* Ollama
* Qdrant
* AI Services

---

## applications

Reserved for:

* Business Applications
* Spring Boot Services
* Future Microservices

# 11. GitOps Architecture

## Overview

NEXO-RAIDEN follows a GitOps operating model.

Git is the authoritative source of truth for platform configuration.

All infrastructure changes originate from Git and are automatically reconciled into Kubernetes through ArgoCD.

---

## GitOps Principles

### Declarative Configuration

Infrastructure is described through manifests.

Examples:

* Namespaces
* Deployments
* Services
* Ingresses
* Certificates
* External Secrets

---

### Version Control

Every platform change is:

* Tracked
* Audited
* Reproducible
* Recoverable

---

### Continuous Reconciliation

ArgoCD continuously compares:

Desired State (Git)

versus

Actual State (Cluster)

If differences exist:

ArgoCD reconciles the platform.

---

## Repository Structure

homelab-gitops/

├── apps/
├── infrastructure/
├── clusters/
├── docs/

---

## Current GitOps Components

### ArgoCD

Purpose:

Continuous Delivery Engine

Responsibilities:

* Synchronization
* Drift Detection
* Self-Healing
* Deployment Automation

---

### GitHub

Purpose:

Source Control

Responsibilities:

* Version Control
* Pull Requests
* Branch Protection

---

# 12. CI/CD Architecture

## Overview

GitHub Actions provides the Continuous Integration layer.

ArgoCD provides the Continuous Delivery layer.

---

## Current Workflow

Engineer
↓
Git Push
↓
GitHub Actions
↓
Validation
↓
Security Scan
↓
GitOps Repository
↓
ArgoCD
↓
Kubernetes

---

## Planned Workflows

### GitOps Validation

Validates:

* Repository structure
* Kubernetes manifests
* Documentation

---

### Security Scanning

Tools:

* Trivy
* Dependency Scanning
* Secret Detection

---

### Future Build Pipeline

Java Service

↓

Build

↓

Test

↓

Container Build

↓

Image Scan

↓

Registry

↓

GitOps Update

↓

ArgoCD Deployment

---

# 13. Security Architecture

## Security Philosophy

Security is integrated into every platform layer.

The architecture follows:

Defense in Depth

---

## Security Domains

### Identity Security

Keycloak

### Secret Security

Vault

### Certificate Security

cert-manager

### Platform Security

Kubernetes

### Supply Chain Security

GitHub Actions

Future:

* Cosign
* SBOM
* OPA
* Falco

---

## Security Layers

User
↓
TLS
↓
Ingress
↓
Keycloak
↓
Application

---

# 14. Identity Architecture

## Identity Provider

Keycloak

Purpose:

Central Authentication and Authorization Platform

---

## Responsibilities

* Authentication
* Authorization
* OIDC
* SSO

---

## Authentication Flow

User
↓
Keycloak
↓
PostgreSQL
↓
Access Token
↓
Application

---

## Future Integrations

* Grafana
* ArgoCD
* Spring Boot Services
* AI Services

---

# 15. Secret Management Architecture

## Overview

Secrets are centrally managed through Vault.

Secrets never originate from Git.

---

## Secret Flow

Vault
↓
ClusterSecretStore
↓
ExternalSecret
↓
Kubernetes Secret
↓
Application

---

## Current Managed Secrets

### PostgreSQL

nexo-raiden-postgresql-secret

---

### Keycloak

nexo-raiden-keycloak-secret

---

## Security Rules

Never:

* Store secrets in Git
* Hardcode passwords
* Commit credentials

---

# 16. Certificate Architecture

## Overview

Certificate management is automated using cert-manager.

---

## Certificate Flow

Ingress
↓
Certificate Request
↓
cert-manager
↓
TLS Secret
↓
HTTPS

---

## Current Issuer

nexo-raiden-selfsigned

---

## Current Certificate

keycloak-nexo-raiden-local-tls

---

## Future Evolution

* Let's Encrypt
* DNS01 Validation
* AWS ACM

---

# 17. Networking Architecture

## Overview

Networking provides secure communication between users, applications, and platform services.

---

## Current Network Flow

User
↓
Ingress NGINX
↓
Service
↓
Pod

---

## Service Discovery

Kubernetes DNS

Example:

nexo-raiden-postgresql.data.svc.cluster.local

---

## Current Public Endpoint

keycloak.nexo-raiden.local

---

## Internal Services

* PostgreSQL
* Vault
* External Secrets
* cert-manager

---

## Future Networking

* Network Policies
* Zero Trust
* AWS VPC
* Route53

---

# 18. Data Architecture

## Overview

The Data Layer provides persistence for platform services.

---

## Current Data Services

### PostgreSQL

Purpose:

Primary Relational Database

Database:

nexo_platform

Administrative User:

nexo_admin

---

## Data Flow

Keycloak
↓
PostgreSQL
↓
Persistent Volume

---

## Backup Strategy

PostgreSQL
↓
pg_dumpall
↓
Compressed Archive

---

## Future Data Services

* RabbitMQ
* Kafka
* Redis
* Qdrant

---

# 19. Observability Architecture

## Overview

Observability provides visibility into platform health and performance.

---

## Current Components

### Prometheus

Metrics Collection

---

### Grafana

Visualization

---

### Node Exporter

Host Metrics

---

## Monitoring Flow

Platform
↓
Prometheus
↓
Grafana
↓
Engineer

---

## Future Components

* Alertmanager
* Loki
* ELK Stack

---

# 20. Messaging Architecture

## Future State

The platform will support event-driven architectures.

---

## RabbitMQ

Purpose:

Reliable Message Delivery

Use Cases:

* Work Queues
* Background Processing
* Service Communication

---

## Kafka

Purpose:

Event Streaming

Use Cases:

* Audit Events
* Analytics
* Event Sourcing

---

# 21. AI Architecture

## Vision

Provide an enterprise-grade AI platform.

---

## Planned Components

### Ollama

LLM Runtime

---

### Qdrant

Vector Database

---

### AI Gateway

Future Service

Responsibilities:

* Prompt Management
* Retrieval
* Auditing

---

## Future AI Flow

User
↓
Application
↓
AI Gateway
↓
Qdrant
↓
Ollama
↓
Response

---

# 22. Platform Request Flows

## Authentication Flow

User
↓
HTTPS
↓
Ingress
↓
Keycloak
↓
PostgreSQL

---

## Secret Delivery Flow

Vault
↓
External Secrets
↓
Kubernetes Secret
↓
Application

---

## Deployment Flow

Engineer
↓
GitHub
↓
GitHub Actions
↓
ArgoCD
↓
Kubernetes

---

## Certificate Flow

Ingress
↓
cert-manager
↓
TLS Secret
↓
HTTPS


# 23. Platform Dependency Matrix

## Purpose

This section documents the dependencies between all platform services.

Understanding dependencies is critical for:

* Troubleshooting
* Change Management
* Incident Response
* Disaster Recovery
* AWS Migration Planning

---

## Dependency Overview

| Component           | Depends On                                 | Criticality |
| ------------------- | ------------------------------------------ | ----------- |
| Ubuntu Server       | Raspberry Pi Hardware                      | Critical    |
| K3s                 | Ubuntu Server                              | Critical    |
| ArgoCD              | K3s                                        | Critical    |
| Vault               | K3s, PVC                                   | Critical    |
| External Secrets    | Vault                                      | Critical    |
| cert-manager        | K3s CRDs                                   | High        |
| PostgreSQL          | PVC, External Secrets                      | Critical    |
| Keycloak            | PostgreSQL, cert-manager, External Secrets | Critical    |
| Prometheus          | K3s                                        | Medium      |
| Grafana             | Prometheus                                 | Medium      |
| GitHub Actions      | GitHub                                     | Medium      |
| RabbitMQ (Future)   | K3s                                        | High        |
| Kafka (Future)      | K3s                                        | High        |
| Qdrant (Future)     | PVC                                        | High        |
| AI Gateway (Future) | Qdrant, Ollama                             | High        |

---

## Critical Service Chain

Ubuntu
↓
K3s
↓
ArgoCD
↓
Vault
↓
External Secrets
↓
PostgreSQL
↓
Keycloak
↓
Applications

Failure at any level impacts all downstream services.

---

# 24. Current Platform Inventory

## Infrastructure Layer

| Component           | Status  |
| ------------------- | ------- |
| Raspberry Pi 5      | Active  |
| Ubuntu Server 24.04 | Active  |
| K3s                 | Healthy |

---

## GitOps Layer

| Component      | Status  |
| -------------- | ------- |
| ArgoCD         | Healthy |
| Git Repository | Active  |

Current Applications:

* nexo-raiden-platform-root
* nexo-raiden-argocd-projects

---

## Security Layer

| Component        | Status  |
| ---------------- | ------- |
| Vault            | Healthy |
| External Secrets | Healthy |
| cert-manager     | Healthy |
| Keycloak         | Healthy |
| TLS Certificates | Healthy |

---

## Data Layer

| Component      | Status  |
| -------------- | ------- |
| PostgreSQL     | Healthy |
| PostgreSQL PVC | Healthy |
| Vault PVC      | Healthy |

---

## Observability Layer

| Component     | Status    |
| ------------- | --------- |
| Prometheus    | Installed |
| Grafana       | Installed |
| Node Exporter | Installed |

---

## CI/CD Layer

Current:

* GitHub

Planned:

* GitHub Actions
* Trivy
* Cosign
* SBOM

---

## AI Layer

Planned:

* Ollama
* Qdrant
* AI Gateway

---

# 25. Operational Model

## Philosophy

The platform follows:

GitOps
+
Infrastructure as Code
+
Security by Design

---

## Platform Change Lifecycle

Engineer
↓
Git Commit
↓
GitHub
↓
GitHub Actions
↓
ArgoCD
↓
Kubernetes
↓
Running Service

---

## Secret Lifecycle

Engineer
↓
Vault
↓
External Secret
↓
Kubernetes Secret
↓
Application

---

## Certificate Lifecycle

Ingress
↓
cert-manager
↓
Certificate
↓
TLS Secret
↓
HTTPS Endpoint

---

## Monitoring Lifecycle

Applications
↓
Prometheus
↓
Grafana
↓
Engineers

---

## Backup Lifecycle

PostgreSQL
↓
Backup Script
↓
Backup Archive

Vault
↓
Backup Script
↓
Backup Archive

---

# 26. Backup Strategy

## Objective

Protect platform state and minimize data loss.

---

## Protected Components

Current:

* PostgreSQL
* Vault

Future:

* Grafana Dashboards
* AI Vector Data
* RabbitMQ Definitions

---

## PostgreSQL Backup

Method:

pg_dumpall

Location:

~/backups/postgresql

Script:

~/scripts/postgresql-backup.sh

Retention:

7 Days

---

## Vault Backup

Method:

File Storage Archive

Location:

~/backups/vault

Script:

~/scripts/vault-backup.sh

Retention:

7 Days

---

## Future Backup Improvements

* Encrypted Backups
* Off-Device Backups
* S3 Storage
* Backup Validation
* Automated Restore Testing

---

# 27. Recovery Strategy

## Recovery Philosophy

Infrastructure is rebuilt through:

GitOps

Data is restored through:

Backups

---

## Recovery Order

1. Ubuntu
2. K3s
3. ArgoCD
4. Vault
5. External Secrets
6. PostgreSQL
7. Keycloak
8. Applications

---

## Recovery Documentation

* cluster-recovery.md
* vault-recovery.md
* postgresql-recovery.md
* backup-strategy.md

---

## Recovery Objectives

RTO:

< 4 Hours

RPO:

< 24 Hours

---

# 28. Security Roadmap

## Current State

Implemented:

* Vault
* External Secrets
* cert-manager
* Keycloak
* TLS

---

## Phase 1

Network Policies

---

## Phase 2

Trivy

---

## Phase 3

Falco

---

## Phase 4

OPA Gatekeeper

---

## Phase 5

Cosign

---

## Phase 6

SBOM Generation

---

# 29. Platform Roadmap

## Foundation Layer

Completed:

* K3s
* ArgoCD
* Vault
* External Secrets
* cert-manager
* PostgreSQL
* Keycloak

---

## Operations Layer

In Progress:

* Backup Automation
* Recovery Documentation

---

## Observability Layer

Planned:

* Alertmanager
* Loki
* ELK

---

## Messaging Layer

Planned:

* RabbitMQ
* Kafka

---

## AI Layer

Planned:

* Ollama
* Qdrant
* AI Gateway

---

## Cloud Layer

Planned:

* AWS
* EKS
* ECR
* Route53
* ACM

---

# 30. AWS Migration Architecture

## Objective

Migrate platform concepts without redesign.

---

## Mapping

| Current         | AWS Future     |
| --------------- | -------------- |
| Raspberry Pi    | EC2            |
| K3s             | EKS            |
| Local PVC       | EBS            |
| Vault           | HCP Vault      |
| PostgreSQL      | RDS PostgreSQL |
| Ingress NGINX   | ALB            |
| Self-Signed TLS | ACM            |
| Local DNS       | Route53        |
| GitHub Actions  | GitHub Actions |

---

## Migration Strategy

Phase 1:

Lift-and-Shift

Phase 2:

Managed Services

Phase 3:

Multi-Region Architecture

---

# 31. Multi-Cloud Evolution

## Vision

Avoid platform lock-in.

---

## Target Providers

* AWS
* Azure
* GCP

---

## Design Principles

* Kubernetes First
* Open Standards
* Portable Workloads
* Cloud-Agnostic CI/CD

---

# 32. Technology Standards

## Operating System

Ubuntu Server 24.04 LTS

---

## Container Platform

K3s

---

## GitOps

ArgoCD

---

## CI/CD

GitHub Actions

---

## Secret Management

Vault

---

## Identity Management

Keycloak

---

## Database

PostgreSQL

---

## Messaging

RabbitMQ
Kafka

---

## Observability

Prometheus
Grafana

---

## AI

Ollama
Qdrant

---

## Cloud

AWS

---

# 33. Documentation Standards

## Documentation Philosophy

Documentation is a platform deliverable.

---

## Required Documentation

Every component must have:

* Architecture Documentation
* Deployment Documentation
* Operational Documentation
* Recovery Documentation

---

## Required Directories

docs/architecture

docs/runbooks

---

## Documentation Requirements

Every document must explain:

* Purpose
* Architecture
* Components
* Dependencies
* Operations
* Security
* Recovery
* Future Evolution

---

## NEXO-RAIDEN Rule

A component is not considered complete until documentation exists.

---

# 34. Platform Summary

NEXO-RAIDEN is a cloud-native Platform Engineering environment designed to demonstrate enterprise-grade engineering practices across:

* Platform Engineering
* Cloud Engineering
* Security Engineering
* DevSecOps
* AI Engineering

The platform currently operates on:

* Raspberry Pi 5
* Ubuntu Server 24.04
* K3s Kubernetes

Core capabilities include:

* GitOps
* Secret Management
* Identity Management
* Certificate Automation
* Database Services
* Backup and Recovery

The platform is designed to evolve toward:

* AWS
* Multi-Cloud
* Event-Driven Architectures
* AI-Native Workloads

while maintaining security, observability, recoverability, and operational excellence as first-class engineering principles.

