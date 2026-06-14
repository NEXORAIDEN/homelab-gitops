# PostgreSQL

## Purpose

PostgreSQL is the primary relational database of the NEXO-RAIDEN platform.

It provides persistent storage for:

- Keycloak
- Future Spring Boot services
- Event-driven applications
- Audit data
- Platform metadata

---

# Business Problem

Applications require persistent storage.

Without PostgreSQL:

- Data is lost when containers restart
- No transactions
- No relational data model
- No enterprise persistence layer

---

# Solution

PostgreSQL provides:

- ACID compliance
- Transactions
- SQL support
- High reliability
- Enterprise adoption

---

# Current Consumers

## Keycloak

Stores:

- Users
- Roles
- Groups
- Sessions
- Realms

---

# Future Consumers

## Account Service

Stores:

- Accounts
- Customers

## Transaction Service

Stores:

- Transactions
- Transfers

## Audit Service

Stores:

- Audit records

---

# Current Status

| Component | Status |
|------------|---------|
| PostgreSQL Pod | Operational |
| PVC | Operational |
| Service | Operational |
| Vault Secret Integration | Operational |
| External Secret | Operational |
