# Identity Architecture

## Document Information

| Attribute         | Value                          |
| ----------------- | ------------------------------ |
| Document Name     | Identity Architecture          |
| Platform          | NEXO-RAIDEN                    |
| Identity Provider | Keycloak                       |
| Protocols         | OIDC, OAuth2, OpenID Connect   |
| Classification    | Internal Platform Architecture |
| Owner             | Platform Engineering           |
| Criticality       | Tier 1                         |

---

# 1. Executive Summary

Identity is one of the most critical platform capabilities within NEXO-RAIDEN.

The Identity Platform provides centralized:

* Authentication
* Authorization
* Single Sign-On (SSO)
* Identity Federation
* Service Authentication
* Access Control

All platform services should ultimately trust Keycloak as the central Identity Provider (IdP).

Identity serves as the security boundary between users, services, and platform resources.

---

# 2. Identity Vision

## Strategic Goal

Provide a unified identity platform for:

* Human Users
* Platform Administrators
* Developers
* Service Accounts
* Applications
* AI Services

---

## Future State

User
↓
Keycloak
↓
OIDC Token
↓
ArgoCD

Grafana

Spring Boot APIs

RabbitMQ

Kafka UI

AI Platform

AWS Applications

One identity.

One login.

One authorization model.

---

# 3. Identity Principles

## Centralized Authentication

Authentication occurs in Keycloak.

Applications should never authenticate users directly.

---

## Federated Identity

Applications trust Keycloak.

Applications do not manage passwords.

---

## Least Privilege

Users receive only required permissions.

---

## Zero Trust

Every request must be authenticated and authorized.

---

## Auditable Access

Authentication and authorization decisions must be traceable.

---

# 4. Identity Architecture Overview

## High-Level Architecture

User
↓
Browser
↓
HTTPS
↓
Ingress NGINX
↓
Keycloak
↓
OIDC Token
↓
Application

---

## Core Components

| Component        | Purpose                |
| ---------------- | ---------------------- |
| Keycloak         | Identity Provider      |
| PostgreSQL       | Identity Storage       |
| cert-manager     | TLS Certificates       |
| Vault            | Secret Management      |
| External Secrets | Secret Synchronization |
| Ingress NGINX    | Secure Access          |

---

# 5. Identity Domains

## Human Identity

Examples:

* Administrators
* Developers
* Operators
* Auditors

---

## Machine Identity

Examples:

* Spring Boot Services
* CI/CD Pipelines
* Future AI Services

---

## Platform Identity

Examples:

* ArgoCD
* Grafana
* RabbitMQ
* Kafka UI

---

# 6. Keycloak Architecture

## Purpose

Central Identity Provider.

---

## Responsibilities

Authentication

Authorization

OIDC

OAuth2

Single Sign-On

Role Management

User Management

Service Accounts

Token Issuance

Session Management

---

## Current Deployment

Namespace:

platform

Application:

nexo-raiden-keycloak

Hostname:

keycloak.nexo-raiden.local

---

## Dependencies

PostgreSQL

Vault

External Secrets

cert-manager

Ingress

---

## Failure Impact

Critical

If Keycloak becomes unavailable:

* Authentication stops
* SSO stops
* API authorization fails
* Platform administration becomes unavailable

---

# 7. Authentication Architecture

## Purpose

Verify identity.

---

## Current Flow

User
↓
Browser
↓
HTTPS
↓
Keycloak
↓
Credential Validation
↓
PostgreSQL
↓
Session Created
↓
Token Issued

---

## Authentication Methods

Current:

Username + Password

Future:

MFA

WebAuthn

Hardware Keys

Federated Login

---

# 8. Authorization Architecture

## Purpose

Determine access rights.

---

## Authorization Flow

User
↓
Authenticated
↓
Roles Loaded
↓
Permissions Evaluated
↓
Access Granted

---

## Authorization Model

Role-Based Access Control (RBAC)

Future:

Attribute-Based Access Control (ABAC)

---

# 9. Realm Architecture

## Current Realm

master

---

## Future Realm Design

nexo-raiden-platform

development

staging

production

---

## Realm Responsibilities

User Management

Role Management

Client Management

Identity Policies

---

# 10. Client Architecture

## Purpose

Applications registered with Keycloak.

---

## Current Clients

Keycloak Admin Console

---

## Future Clients

Grafana

ArgoCD

Transaction Service

Account Service

Audit Service

RabbitMQ Management

Kafka UI

AI Gateway

---

# 11. Role Architecture

## Platform Administrator

Responsibilities:

Full Platform Access

---

## Security Administrator

Responsibilities:

Identity Security

Secrets

Certificates

---

## Platform Operator

Responsibilities:

Operations

Monitoring

Recovery

---

## Developer

Responsibilities:

Application Development

---

## Auditor

Responsibilities:

Read-Only Access

Audit Review

---

## Viewer

Responsibilities:

Dashboard Access

Read-Only Visibility

---

# 12. Group Architecture

## Purpose

Simplify authorization.

---

## Future Groups

Platform Engineering

Security Engineering

Developers

Operations

Auditors

---

# 13. Token Architecture

## Access Token

Purpose:

API Authorization

---

## ID Token

Purpose:

User Identity Information

---

## Refresh Token

Purpose:

Session Renewal

---

## Token Lifecycle

Login
↓
Token Issued
↓
Token Used
↓
Token Expires
↓
Refresh Token
↓
New Access Token

---

# 14. Service Account Architecture

## Purpose

Machine-to-Machine Authentication.

---

## Example Flow

transaction-service
↓
Client Credentials
↓
Keycloak
↓
Access Token
↓
audit-service

---

## Benefits

No Shared Passwords

Auditable Access

Least Privilege

---

# 15. Platform Integrations

## Grafana

Future:

OIDC Authentication

---

## ArgoCD

Future:

OIDC Authentication

---

## Spring Boot Services

Future:

Resource Server

JWT Validation

---

## RabbitMQ

Future:

Keycloak Integration

---

## Kafka UI

Future:

OIDC Authentication

---

# 16. Security Model

## Security Controls

TLS

OIDC

RBAC

Service Accounts

Secret Management

---

## Trust Boundary

Internet
↓
Ingress
↓
Keycloak
↓
Application

---

## Security Objectives

Prevent Unauthorized Access

Centralize Identity

Reduce Credential Sprawl

---

# 17. User Lifecycle Management

## User Creation

Administrator
↓
Keycloak
↓
User Created

---

## Role Assignment

User
↓
Group
↓
Role

---

## Deactivation

User Disabled

Sessions Revoked

Access Removed

---

# 18. Audit and Compliance

## Auditable Events

Login

Logout

Failed Login

Role Changes

User Creation

User Deletion

Client Creation

---

## Future Audit Pipeline

Keycloak
↓
Kafka
↓
Audit Service
↓
Analytics

---

# 19. Failure Scenarios

## Keycloak Failure

Impact:

Authentication unavailable.

Recovery Priority:

Critical

---

## PostgreSQL Failure

Impact:

Identity platform unavailable.

Recovery Priority:

Critical

---

## Certificate Failure

Impact:

HTTPS unavailable.

Recovery Priority:

High

---

# 20. Identity Recovery

## Recovery Sequence

1. PostgreSQL
2. Keycloak
3. Clients
4. Users
5. Integrations

---

## Related Runbooks

cluster-recovery.md

postgresql-recovery.md

vault-recovery.md

---

# 21. AWS Identity Mapping

| Current          | AWS Future                     |
| ---------------- | ------------------------------ |
| Keycloak         | Keycloak on EKS                |
| Local Users      | IAM Identity Center            |
| OIDC             | IAM Federation                 |
| RBAC             | IAM Roles                      |
| Service Accounts | IAM Roles for Service Accounts |

---

# 22. Future Evolution

## Phase 1

Keycloak

---

## Phase 2

Grafana SSO

---

## Phase 3

ArgoCD SSO

---

## Phase 4

Spring Boot OIDC

---

## Phase 5

Service Account Architecture

---

## Phase 6

Multi-Factor Authentication

---

## Phase 7

AWS Federation

---

# 23. Related Documentation

platform-topology.md

security.md

networking.md

gitops.md

ci-cd.md

observability.md

---

# 24. Summary

Identity is the security foundation of the NEXO-RAIDEN platform.

Keycloak provides centralized authentication, authorization, token issuance, and access control for users, applications, and future cloud-native services.

The identity architecture is designed to scale from a Raspberry Pi homelab deployment to enterprise-grade AWS and multi-cloud environments while maintaining security, auditability, and operational simplicity.
