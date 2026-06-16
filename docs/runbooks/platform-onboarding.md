# Platform Onboarding Guide

## Document Information

| Attribute      | Value                                                                     |
| -------------- | ------------------------------------------------------------------------- |
| Document Name  | Platform Onboarding Guide                                                 |
| Platform       | NEXO-RAIDEN                                                               |
| Classification | Internal Runbook                                                          |
| Audience       | Platform Engineers, DevOps Engineers, Cloud Engineers, Security Engineers |
| Owner          | Platform Engineering                                                      |
| Criticality    | High                                                                      |

---

# 1. Welcome to NEXO-RAIDEN

## Introduction

Welcome to the NEXO-RAIDEN Platform Engineering environment.

NEXO-RAIDEN is a cloud-native engineering platform focused on:

* Platform Engineering
* Cloud Engineering
* Security Engineering
* DevSecOps
* AI Engineering

The platform serves two purposes:

1. Production-like engineering environment
2. Enterprise portfolio demonstrating modern engineering practices

---

## Engineering Philosophy

Every platform capability must be:

* Secure
* Observable
* Automated
* Documented
* Recoverable
* Repeatable

---

## Core Principles

### Git Is The Source Of Truth

Infrastructure state originates from Git.

---

### Documentation Is A Deliverable

A component is not complete until documentation exists.

---

### Security By Design

Every component must support:

* Authentication
* Authorization
* TLS
* Secret Management

---

### Recovery By Design

Every component must be recoverable.

---

# 2. Platform Overview

## Architecture Domains

The platform consists of the following major domains:

### Infrastructure

Provides:

* Compute
* Storage
* Operating System

---

### Kubernetes

Provides:

* Container Orchestration
* Service Discovery
* Scheduling

---

### GitOps

Provides:

* Continuous Delivery
* Reconciliation
* Drift Detection

---

### Security

Provides:

* Identity
* Secrets
* Certificates

---

### Data

Provides:

* Persistent Storage
* Databases

---

### Observability

Provides:

* Monitoring
* Metrics
* Dashboards

---

### Messaging

Provides:

* Asynchronous Communication
* Event Streaming

---

### AI Platform

Provides:

* Local LLMs
* Vector Search
* RAG Workloads

---

# 3. Current Platform Stack

## Infrastructure Layer

* Raspberry Pi 5
* Ubuntu Server 24.04 LTS

---

## Kubernetes Layer

* K3s

---

## GitOps Layer

* GitHub
* ArgoCD

---

## Security Layer

* Vault
* External Secrets
* cert-manager
* Keycloak

---

## Data Layer

* PostgreSQL

---

## Observability Layer

* Prometheus
* Grafana
* Node Exporter

---

## Future Services

* RabbitMQ
* Kafka
* Qdrant
* Ollama
* AI Gateway

---

# 4. Architecture Documentation Map

Every engineer should become familiar with:

## Master Architecture

docs/architecture/platform-topology.md

---

## Security

docs/architecture/security.md

docs/architecture/identity.md

---

## Operations

docs/operations/platform-operations.md

docs/operations/platform-slos.md

---

## Recovery

docs/runbooks/disaster-recovery.md

docs/runbooks/security-incident-response.md

---

# 5. Repository Structure

## High-Level Layout

homelab-gitops/

├── apps/
├── infrastructure/
├── docs/
├── scripts/
├── clusters/

---

## apps/

Contains ArgoCD Applications.

Examples:

* vault.yaml
* keycloak.yaml
* postgresql.yaml

---

## infrastructure/

Contains Kubernetes manifests.

Examples:

* security/
* platform/
* data/
* monitoring/

---

## docs/

Contains:

* Architecture Documentation
* Operational Documentation
* Runbooks

---

## scripts/

Contains:

* Backup Scripts
* Automation Scripts
* Recovery Scripts

---

# 6. Namespace Strategy

## security

Contains:

* Vault
* External Secrets
* cert-manager

---

## platform

Contains:

* Keycloak
* Ingress

---

## data

Contains:

* PostgreSQL

---

## monitoring

Contains:

* Prometheus
* Grafana

---

## ai

Reserved for:

* Ollama
* Qdrant

---

# 7. Git Workflow

## Branch Strategy

main

feature/*

hotfix/*

---

## Feature Workflow

Engineer
↓
Feature Branch
↓
Commit
↓
Pull Request
↓
Review
↓
Merge

---

## Commit Standards

Commits should be:

* Small
* Focused
* Descriptive

Example:

feat(keycloak): add oidc configuration

---

# 8. GitOps Workflow

## Deployment Flow

Engineer
↓
Git Commit
↓
GitHub
↓
ArgoCD
↓
Kubernetes

---

## Desired State Model

Git contains:

Desired State

ArgoCD ensures:

Actual State

matches

Desired State

---

## Drift Management

Configuration drift is automatically detected.

---

# 9. Security Standards

## Secret Management

Secrets belong in:

Vault

Never:

* Git
* Source Code
* Documentation

---

## Identity

Authentication belongs in:

Keycloak

Applications should trust Keycloak.

---

## Certificates

Managed by:

cert-manager

---

## Security Reviews

All new services must include:

* Secret Strategy
* Authentication Strategy
* TLS Strategy

---

# 10. Development Environment Setup

## Required Tools

kubectl

git

helm

argocd CLI

vault CLI

jq

curl

---

## Verify Access

kubectl get nodes

kubectl get pods -A

kubectl get applications -n argocd

---

# 11. Daily Operational Activities

Verify:

* Node Health
* Pod Health
* GitOps Health
* Storage Health
* Certificate Health

---

## Daily Commands

kubectl get nodes

kubectl get pods -A

kubectl get applications -n argocd

---

# 12. Weekly Operational Activities

Review:

* Backups
* Certificates
* Storage Usage
* Documentation

---

# 13. Incident Management

## Infrastructure Incidents

Follow:

disaster-recovery.md

---

## Security Incidents

Follow:

security-incident-response.md

---

## Escalation Philosophy

Identify
↓
Contain
↓
Investigate
↓
Recover
↓
Document

---

# 14. Platform Recovery Expectations

Every engineer should understand:

* Cluster Recovery
* Vault Recovery
* PostgreSQL Recovery
* Keycloak Recovery

---

## Recovery Documentation

cluster-recovery.md

vault-recovery.md

postgresql-recovery.md

---

# 15. Engineering Standards

## Documentation Standard

Every component requires:

* Architecture Document
* Operational Guide
* Recovery Guide

---

## Security Standard

Every component requires:

* Authentication
* Authorization
* Secret Management

---

## Observability Standard

Every component requires:

* Metrics
* Dashboards
* Monitoring

---

# 16. First 30-Day Learning Path

## Week 1

Read:

* platform-topology.md
* security.md
* identity.md

Understand:

* Kubernetes
* GitOps
* Security

---

## Week 2

Learn:

* Vault
* External Secrets
* Keycloak

Perform:

* Secret Creation
* Certificate Validation

---

## Week 3

Learn:

* PostgreSQL
* Prometheus
* Grafana

Perform:

* Backup Validation
* Recovery Exercises

---

## Week 4

Learn:

* CI/CD
* Messaging Architecture
* AI Architecture

Perform:

* End-to-End Platform Review

---

# 17. Success Criteria

A newly onboarded engineer should be able to:

* Explain platform architecture
* Deploy a service
* Troubleshoot failures
* Restore backups
* Follow security standards
* Operate the platform safely

within the first month.

---

# 18. Related Documentation

Architecture:

* platform-topology.md
* security.md
* identity.md
* observability.md
* messaging.md
* ai-platform.md

Operations:

* platform-operations.md
* platform-slos.md

Runbooks:

* disaster-recovery.md
* security-incident-response.md

---

# 19. Summary

The onboarding guide provides a structured path for engineers to understand, operate, secure, and evolve the NEXO-RAIDEN platform.

It serves as the primary entry point into the platform knowledge base and should be reviewed by every engineer before making platform changes.
