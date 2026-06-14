# Vault Operations

## Check Status

kubectl exec \
-n security \
vault-0 \
-- vault status

---

## List Secret Paths

kubectl exec \
-n security \
vault-0 \
-- vault kv list nexo

---

## Read Secret

kubectl exec \
-n security \
vault-0 \
-- vault kv get \
nexo/platform/keycloak

---

## Create Secret

kubectl exec \
-n security \
vault-0 \
-- vault kv put \
nexo/platform/keycloak \
KEY=value

---

## Restart Vault

kubectl rollout restart \
statefulset/vault \
-n security

---

## Verify External Secrets

kubectl get externalsecret -A
