# PostgreSQL Implementation

## ArgoCD Application

File:

apps/postgresql.yaml

Purpose:

Deploy PostgreSQL through GitOps.

---

# Namespace

data

---

# Files

## deployment.yaml

Location:

infrastructure/data/postgresql/deployment.yaml

Purpose:

Creates PostgreSQL container.

---

## service.yaml

Location:

infrastructure/data/postgresql/service.yaml

Purpose:

Provides stable DNS endpoint.

---

## pvc.yaml

Location:

infrastructure/data/postgresql/pvc.yaml

Purpose:

Persists database files.

---

## external-secret.yaml

Location:

infrastructure/data/postgresql/external-secret.yaml

Purpose:

Synchronizes Vault secrets.

---

# Database Endpoint

nexo-raiden-postgresql.data.svc.cluster.local

Port:

5432

---

# Verification

kubectl get pods -n data

kubectl get pvc -n data

kubectl get secret -n data

kubectl get externalsecret -n data
