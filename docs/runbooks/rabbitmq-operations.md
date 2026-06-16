# RabbitMQ Operations Runbook

## Purpose

Operational procedures for managing RabbitMQ within the NEXO-RAIDEN platform.

---

# Health Checks

## Verify ArgoCD

```bash
kubectl get applications -n argocd | grep rabbit
```

Expected:

```text
Synced
Healthy
```

---

## Verify Pod

```bash
kubectl get pods -n messaging
```

Expected:

```text
1/1 Running
```

---

## Verify Services

```bash
kubectl get svc -n messaging
```

---

## Verify Storage

```bash
kubectl get pvc -n messaging
```

Expected:

```text
Bound
```

---

# Secret Validation

Verify External Secret:

```bash
kubectl get externalsecret -n messaging
```

Expected:

```text
SecretSynced
True
```

Verify Kubernetes Secret:

```bash
kubectl get secret nexo-raiden-rabbitmq-secret -n messaging
```

---

# RabbitMQ Login

Retrieve credentials:

```bash
kubectl get secret nexo-raiden-rabbitmq-secret \
  -n messaging \
  -o jsonpath="{.data.RABBITMQ_PASSWORD}" | base64 -d
```

Retrieve service endpoint:

```bash
kubectl get svc -n messaging
```

---

# Troubleshooting

## Pod Stuck Pending

Check:

```bash
kubectl describe pod -n messaging
```

Verify:

* Storage
* Secrets
* Scheduling

---

## Image Pull Errors

Check:

```bash
kubectl describe pod -n messaging
```

Verify image repository.

Current platform image:

```text
bitnamilegacy/rabbitmq
```

---

## Secret Errors

Check:

```bash
kubectl describe externalsecret \
  nexo-raiden-rabbitmq-secret \
  -n messaging
```

Verify:

* Vault Secret Exists
* Property Names Match
* ClusterSecretStore Healthy

---

## Vault Validation

```bash
kubectl exec -it -n security vault-0 -- sh
```

Verify:

```bash
vault kv get nexo/messaging/rabbitmq
```

Expected:

```text
RABBITMQ_PASSWORD
```

---

# Restart Procedure

```bash
kubectl rollout restart statefulset \
  nexo-raiden-rabbitmq \
  -n messaging
```

---

# Recovery Procedure

Delete pod:

```bash
kubectl delete pod \
  nexo-raiden-rabbitmq-0 \
  -n messaging
```

Kubernetes automatically recreates the pod.

---

# Backup Requirements

Back up:

* GitOps repository
* Vault secrets
* RabbitMQ definitions
* Persistent volume data

---

# Escalation Matrix

| Severity | Response              |
| -------- | --------------------- |
| P1       | Messaging unavailable |
| P2       | Queue backlog growth  |
| P3       | Monitoring alerts     |
| P4       | Documentation updates |

---

# Platform Standards

* GitOps only
* No manual configuration
* Secrets managed through Vault
* Monitoring mandatory
* Runbooks mandatory
* Recovery tested quarterly
