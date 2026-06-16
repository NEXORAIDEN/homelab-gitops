# RabbitMQ Messaging Platform

## Document Information

| Field          | Value                |
| -------------- | -------------------- |
| Platform       | NEXO-RAIDEN          |
| Component      | RabbitMQ             |
| Version        | 1.0                  |
| Status         | Production Ready     |
| Owner          | Platform Engineering |
| Classification | Internal             |
| Last Updated   | 2026-06-16           |

---

# Purpose

RabbitMQ provides the event-driven messaging backbone for the NEXO-RAIDEN platform.

It enables asynchronous communication between platform services, decouples microservices, improves resiliency, and supports scalable event-driven architectures.

RabbitMQ is deployed using GitOps through ArgoCD and managed as part of the Kubernetes platform foundation.

---

# Business Objectives

The messaging platform provides:

* Reliable service-to-service communication
* Event-driven architecture support
* Asynchronous workload processing
* Audit event distribution
* Notification workflows
* AI workflow orchestration
* Platform integration events

---

# Architecture Overview

```text
+--------------------+
| Spring Boot Apps   |
+---------+----------+
          |
          v
+--------------------+
| RabbitMQ Cluster   |
+---------+----------+
          |
          +--------------------+
          |                    |
          v                    v

 Audit Events          Business Events
 Notifications         AI Workflows
 Integrations          Async Processing
```

---

# Platform Integration

RabbitMQ integrates with:

| Component            | Purpose                 |
| -------------------- | ----------------------- |
| ArgoCD               | GitOps Deployment       |
| Vault                | Secret Storage          |
| External Secrets     | Secret Synchronization  |
| Prometheus           | Metrics Collection      |
| Grafana              | Visualization           |
| Kubernetes           | Runtime Platform        |
| Spring Boot Services | Producers and Consumers |

---

# Deployment Architecture

Namespace:

```text
messaging
```

Primary Resource:

```text
StatefulSet
```

Storage:

```text
Persistent Volume Claim
StorageClass: local-path
```

Persistence Size:

```text
5Gi
```

---

# Security Architecture

Authentication is managed through Vault.

Secrets are never stored in Git repositories.

Flow:

```text
Vault
  ↓
External Secret
  ↓
Kubernetes Secret
  ↓
RabbitMQ
```

---

# High Availability Strategy

Current Environment:

```text
Single Node
```

Future Production Design:

```text
3 Node RabbitMQ Cluster
```

Benefits:

* Node redundancy
* Quorum queues
* Reduced downtime
* Improved fault tolerance

---

# Monitoring

Metrics exported through:

```text
rabbitmq_prometheus
```

Collected by:

```text
Prometheus
```

Visualized by:

```text
Grafana
```

Key Metrics:

* Queue depth
* Message publish rate
* Message consume rate
* Connection count
* Channel count
* Memory usage
* Disk usage

---

# Disaster Recovery

Recovery Components:

* Persistent Volumes
* Vault Secrets
* GitOps Configuration
* RabbitMQ Definitions

Recovery Target:

| Metric | Target       |
| ------ | ------------ |
| RTO    | < 30 Minutes |
| RPO    | < 15 Minutes |

---

# Event Design Standards

## Exchange Types

### Direct Exchange

Used for:

* Point-to-point routing

### Topic Exchange

Used for:

* Business domain events

### Fanout Exchange

Used for:

* Broadcast events

### Headers Exchange

Used for:

* Advanced routing scenarios

---

# Future Roadmap

Phase 1

* Single-node RabbitMQ

Phase 2

* TLS Encryption

Phase 3

* Quorum Queues

Phase 4

* Multi-node Cluster

Phase 5

* Cross-cluster Federation

Phase 6

* Multi-cloud Messaging

---

# References

* GitOps Architecture
* Vault Architecture
* Platform Topology
* Observability Architecture
* Disaster Recovery Runbook
