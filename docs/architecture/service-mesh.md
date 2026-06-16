# NEXO-RAIDEN RabbitMQ Operations Runbook

**Document ID:** NEXO-RAIDEN-RUNBOOK-RMQ-001

**Version:** 1.0

**Classification:** Internal

**Owner:** Platform Engineering

**Environment:** Raspberry Pi 5 Kubernetes Lab (K3s)

**Last Updated:** 2026-06-16

---

# 1. Purpose

This document defines the operational procedures for RabbitMQ within the NEXO-RAIDEN platform.

The objective is to provide:

* Operational standards
* Monitoring procedures
* Troubleshooting guidance
* Recovery procedures
* Security controls
* Disaster recovery instructions

---

# 2. Platform Overview

## Component

RabbitMQ

## Namespace

messaging

## Deployment Model

GitOps (ArgoCD)

## Secret Management

Vault + External Secrets Operator

## Monitoring

Prometheus + Grafana

## Storage

PersistentVolumeClaim (local-path)

## Availability

Single Node

---

# 3. Architecture

Application Services

↓

RabbitMQ Exchange

↓

RabbitMQ Queue

↓

Consumers

↓

Audit / Monitoring

---

# 4. Current Deployment

## Kubernetes Resources

StatefulSet

```bash
kubectl get statefulset -n messaging
```

Pod

```bash
kubectl get pods -n messaging
```

Services

```bash
kubectl get svc -n messaging
```

Persistent Storage

```bash
kubectl get pvc -n messaging
```

Secrets

```bash
kubectl get secret -n messaging
```

External Secrets

```bash
kubectl get externalsecret -n messaging
```

---

# 5. Health Verification

## Verify Pod Status

```bash
kubectl get pods -n messaging
```

Expected:

```text
READY   STATUS
1/1     Running
```

---

## Verify RabbitMQ Services

```bash
kubectl get svc -n messaging
```

Expected:

```text
nexo-raiden-rabbitmq
nexo-raiden-rabbitmq-headless
```

---

## Verify Persistent Storage

```bash
kubectl get pvc -n messaging
```

Expected:

```text
STATUS
Bound
```

---

# 6. RabbitMQ Management Access

## Port Forward

```bash
kubectl port-forward svc/nexo-raiden-rabbitmq \
-n messaging \
15672:15672
```

---

## Web Console

URL

```text
http://localhost:15672
```

---

## Credentials

Username:

```text
nexo_admin
```

Password:

```text
Vault Managed
```

Never store passwords in Git.

---

# 7. Vault Integration

## Secret Location

```text
nexo/data/messaging/rabbitmq
```

Required Key

```text
RABBITMQ_PASSWORD
```

---

## Verify Secret

```bash
kubectl get externalsecret \
nexo-raiden-rabbitmq-secret \
-n messaging
```

Expected

```text
SecretSynced
```

---

## Verify Kubernetes Secret

```bash
kubectl get secret \
nexo-raiden-rabbitmq-secret \
-n messaging
```

Expected

```text
Opaque
```

---

# 8. Monitoring

## Prometheus

Verify ServiceMonitor

```bash
kubectl get servicemonitor -A
```

---

## Metrics Endpoint

```text
:9419/metrics
```

---

## Key Metrics

### Queue Depth

```text
rabbitmq_queue_messages
```

### Connections

```text
rabbitmq_connections
```

### Consumers

```text
rabbitmq_consumers
```

### Published Messages

```text
rabbitmq_channel_messages_published_total
```

### Acknowledged Messages

```text
rabbitmq_channel_messages_confirmed_total
```

---

# 9. Common Incidents

## Incident 1

ImagePullBackOff

Symptoms

```text
Init:ImagePullBackOff
```

Diagnosis

```bash
kubectl describe pod \
nexo-raiden-rabbitmq-0 \
-n messaging
```

Resolution

Verify image repository and chart version.

---

## Incident 2

Secret Missing

Symptoms

```text
MountVolume.SetUp failed
```

Diagnosis

```bash
kubectl get secret \
nexo-raiden-rabbitmq-secret \
-n messaging
```

Resolution

Verify:

* Vault secret exists
* ClusterSecretStore healthy
* ExternalSecret synced

---

## Incident 3

PVC Not Bound

Diagnosis

```bash
kubectl get pvc -n messaging
```

Resolution

Verify:

* local-path provisioner
* StorageClass
* Available disk space

---

# 10. Backup Procedure

## Export Definitions

```bash
kubectl port-forward \
svc/nexo-raiden-rabbitmq \
-n messaging \
15672:15672
```

Login:

```text
RabbitMQ UI
```

Export:

```text
Definitions → Export Definitions
```

Store:

```text
Git Repository Backup Folder
```

---

# 11. Recovery Procedure

## Restore Secret

Verify Vault contains:

```text
RABBITMQ_PASSWORD
```

---

## Restore Deployment

```bash
kubectl delete pod \
nexo-raiden-rabbitmq-0 \
-n messaging
```

StatefulSet recreates automatically.

---

## Full Reconciliation

```bash
kubectl annotate application \
nexo-raiden-rabbitmq \
-n argocd \
argocd.argoproj.io/refresh=hard \
--overwrite
```

---

# 12. Security Controls

Mandatory Controls

* Secrets stored only in Vault
* No plaintext passwords in Git
* GitOps-only deployment
* Prometheus monitoring enabled
* Persistent storage enabled
* Least privilege access

Future Controls

* TLS
* Certificate Manager integration
* OIDC Authentication
* Network Policies
* Quorum Queues

---

# 13. Operational Acceptance Criteria

RabbitMQ is considered healthy when:

✓ ArgoCD Application = Synced

✓ ArgoCD Health = Healthy

✓ Pod Status = Running

✓ PVC = Bound

✓ ExternalSecret = SecretSynced

✓ Secret Present

✓ Metrics Available

✓ Management UI Accessible

✓ No CrashLoopBackOff Events

---

# 14. Current Production Baseline

Date: 2026-06-16

Application:

nexo-raiden-rabbitmq

Status:

Synced

Health:

Healthy

Storage:

5 GiB Persistent Volume

Monitoring:

Enabled

Secrets:

Vault Managed

Deployment:

GitOps Managed

Platform:

NEXO-RAIDEN Kubernetes Lab
