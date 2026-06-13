# NEXO-RAIDEN GitOps Platform

GitOps repository for the NEXO-RAIDEN Raspberry Pi 5 platform lab.

## Platform Stack

- K3s
- ArgoCD
- Prometheus
- Grafana
- Node Exporter
- ELK Stack
- Keycloak
- PostgreSQL
- RabbitMQ / Kafka
- Vault
- Trivy
- OPA / Gatekeeper
- Cosign
- Ollama
- Qdrant
- RAG Service
- Java 21 / Spring Boot 3 platform services

## Architecture

See: `docs/diagrams/platform-architecture.mmd`

## GitOps Root

Root ArgoCD application:

```text
nexo-raiden-platform-root
