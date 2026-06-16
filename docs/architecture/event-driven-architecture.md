# Event-Driven Architecture

**Document ID:** NEXO-RAIDEN-ARCH-EVENT-001
**Version:** 1.0
**Status:** Approved
**Classification:** Internal
**Owner:** Platform Engineering
**Last Updated:** 2026-06-16

---

# 1. Executive Summary

The NEXO-RAIDEN Event-Driven Architecture (EDA) establishes the standard for asynchronous communication across platform services.

RabbitMQ serves as the strategic messaging backbone enabling:

* Service decoupling
* Asynchronous processing
* Reliable event delivery
* Audit event distribution
* AI workflow orchestration
* Cross-domain integrations
* Future cloud-native scaling

The architecture is designed according to modern platform engineering principles and supports future migration to AWS, multi-cloud, and distributed Kubernetes environments.

---

# 2. Business Objectives

## Primary Objectives

### Scalability

Support increasing transaction volume without tight service coupling.

### Reliability

Prevent cascading failures between services.

### Auditability

Guarantee traceability of business events.

### Extensibility

Allow new services to subscribe to events without modifying existing producers.

### Automation

Enable AI and workflow orchestration through event-driven triggers.

---

# 3. Architectural Principles

## AP-01 Event First

Services communicate through events whenever possible.

## AP-02 Loose Coupling

Producers must not know implementation details of consumers.

## AP-03 Immutable Events

Published events must never be modified.

## AP-04 Contract Driven

Events are versioned and documented.

## AP-05 Observable Messaging

All queues and exchanges must be monitored.

## AP-06 Secure by Default

Secrets managed through Vault.

---

# 4. Platform Architecture

```text
┌───────────────────────────────────────────────┐
│                Spring Boot APIs               │
└──────────────────────┬────────────────────────┘
                       │
                       ▼
┌───────────────────────────────────────────────┐
│                  RabbitMQ                      │
│             Event Distribution Layer           │
└───────┬────────────┬────────────┬─────────────┘
        │            │            │
        ▼            ▼            ▼

 Account      Transaction      Audit
 Service      Service          Service

        ▼            ▼            ▼

 Notifications   Analytics   AI Workflows
```

---

# 5. Messaging Domains

## Platform Domain

Platform lifecycle events.

Examples:

```text
platform.user.created
platform.user.updated
platform.user.deleted
```

---

## Identity Domain

Authentication and authorization events.

Examples:

```text
identity.user.login
identity.user.logout
identity.role.assigned
```

---

## Banking Domain

Financial transaction events.

Examples:

```text
banking.account.created
banking.transaction.created
banking.transaction.completed
```

---

## Security Domain

Security-related events.

Examples:

```text
security.login.failed
security.vault.unsealed
security.policy.updated
```

---

## AI Domain

AI orchestration events.

Examples:

```text
ai.workflow.started
ai.workflow.completed
ai.document.processed
```

---

# 6. Exchange Standards

## Topic Exchange

Primary exchange type.

Naming convention:

```text
<nexo-domain>-exchange
```

Examples:

```text
banking-exchange
identity-exchange
audit-exchange
security-exchange
```

---

## Direct Exchange

Used for point-to-point routing.

---

## Fanout Exchange

Used for broadcasting events.

---

# 7. Queue Standards

## Naming Convention

```text
<service>.<purpose>.queue
```

Examples:

```text
audit.event.queue
transaction.processing.queue
notification.email.queue
ai.workflow.queue
```

---

# 8. Routing Key Standards

Format:

```text
domain.entity.action
```

Examples:

```text
banking.transaction.created
banking.transaction.completed
identity.user.created
identity.user.deleted
security.login.failed
```

---

# 9. Event Contract Standard

All events must contain:

```json
{
  "eventId": "uuid",
  "eventVersion": "1.0",
  "eventType": "transaction.created",
  "timestamp": "2026-06-16T12:00:00Z",
  "source": "transaction-service",
  "correlationId": "uuid",
  "payload": {}
}
```

---

# 10. Reliability Requirements

## Delivery

At-Least-Once Delivery

## Durability

All business queues must be durable.

## Persistence

Persistent messages mandatory.

## Dead Letter Queues

Mandatory for all production queues.

Example:

```text
transaction.queue
transaction.queue.dlq
```

---

# 11. Security Architecture

## Authentication

RabbitMQ internal user management.

Current:

```text
Vault
 ↓
External Secret
 ↓
RabbitMQ
```

Future:

```text
Keycloak
 ↓
OIDC
 ↓
RabbitMQ
```

---

# 12. Observability

Metrics collected by:

```text
Prometheus
```

Visualized through:

```text
Grafana
```

Alerting:

```text
Alertmanager
```

Key metrics:

* Queue depth
* Consumer lag
* Message throughput
* Connection count
* Channel count
* Memory utilization
* Disk utilization

---

# 13. Disaster Recovery

## Recovery Objectives

| Metric | Target       |
| ------ | ------------ |
| RTO    | < 30 Minutes |
| RPO    | < 15 Minutes |

---

# 14. Future Roadmap

## Phase 1

Single-node RabbitMQ

Status: Completed

## Phase 2

TLS Encryption

Planned

## Phase 3

Quorum Queues

Planned

## Phase 4

Multi-node RabbitMQ Cluster

Planned

## Phase 5

Cross-cluster Federation

Planned

## Phase 6

AWS Multi-Region Messaging

Planned

---

# 15. Governance

All platform teams must comply with:

* GitOps deployment model
* Vault secret management
* Event contract standards
* Monitoring requirements
* Disaster recovery procedures
* Documentation standards

Non-compliant messaging components are not permitted in the NEXO-RAIDEN platform.

---

# 16. References

* platform-topology.md
* gitops.md
* identity.md
* observability.md
* rabbitmq.md
* disaster-recovery.md
* rabbitmq-operations.md
