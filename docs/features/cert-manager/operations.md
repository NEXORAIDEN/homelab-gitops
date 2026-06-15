# cert-manager Operations

## Verify Pods

kubectl get pods -n security

---

## Verify ClusterIssuer

kubectl get clusterissuer

---

## Verify Certificates

kubectl get certificate -A

---

## Describe Certificate

kubectl describe certificate \
keycloak-nexo-raiden-local-tls \
-n platform

---

## Verify TLS Secret

kubectl get secret \
keycloak-nexo-raiden-local-tls \
-n platform

---

## View Logs

kubectl logs \
-n security \
deploy/cert-manager

---

## Restart cert-manager

kubectl rollout restart \
deployment/cert-manager \
-n security
