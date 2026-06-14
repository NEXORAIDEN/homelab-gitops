# External Secrets Operations

## Verify Operator

kubectl get pods -n security

---

## Verify Secret Stores

kubectl get clustersecretstore

---

## Verify External Secrets

kubectl get externalsecret -A

---

## Describe External Secret

kubectl describe externalsecret \
nexo-raiden-keycloak-secret \
-n platform

---

## Verify Generated Secret

kubectl get secret \
nexo-raiden-keycloak-secret \
-n platform

---

## View Operator Logs

kubectl logs \
-n security \
deploy/external-secrets

---

## Force Refresh

kubectl annotate externalsecret \
nexo-raiden-keycloak-secret \
force-sync=$(date +%s)
