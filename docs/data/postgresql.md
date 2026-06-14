# PostgreSQL

## Purpose

Primary relational database.

## Why We Need It

Stores:

- Keycloak data
- Application data
- Audit records

## Namespace

data

## Files

apps/postgresql.yaml

## Storage

PersistentVolumeClaim

## Verification

kubectl get pods -n data

kubectl get pvc -n data

## Consumers

- Keycloak
- Spring Boot Services
