# Secret Management Strategy

## Purpose

Centralize all platform credentials in Vault.

## Scope

- Keycloak
- PostgreSQL
- RabbitMQ
- Kafka
- Grafana
- Spring Boot Services

## Secret Flow

Vault
↓
External Secrets
↓
Kubernetes Secret
↓
Application

## Benefits

- No secrets in Git
- Secret rotation
- Auditing
- Cloud-ready architecture

## Future

- Vault Kubernetes Auth
- AWS Secrets Manager
- Automatic rotation
