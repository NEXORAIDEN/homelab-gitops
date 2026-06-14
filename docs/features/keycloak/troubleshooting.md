# Keycloak Troubleshooting

## 503 Service Unavailable

Cause:

Backend service unavailable.

Verification:

kubectl get svc -n platform

kubectl get endpoints -n platform

---

## HTTP Redirect Missing Port

Symptom:

Redirects to incorrect URL.

Cause:

KC_HOSTNAME missing port.

Fix:

KC_HOSTNAME=https://keycloak.nexo-raiden.local:30443

---

## TLS Certificate Missing

Verification:

kubectl get certificate -n platform

kubectl get secret keycloak-nexo-raiden-local-tls -n platform

---

## DNS Failure

Error:

DNS_PROBE_FINISHED_NXDOMAIN

Cause:

Hosts file missing entry.

Fix:

Add:

192.168.178.200 keycloak.nexo-raiden.local

---

## Login Failure

Verify:

Vault Secret

External Secret

Database Connectivity

PostgreSQL Pod Status
