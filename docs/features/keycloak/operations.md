# Keycloak Operations

## Verify Deployment

kubectl get pods -n platform

---

## Verify Ingress

kubectl get ingress -n platform

---

## Verify TLS

kubectl get certificate -n platform

---

## Verify Secret

kubectl get externalsecret -n platform

kubectl get secret -n platform

---

## View Logs

kubectl logs \
-n platform \
deploy/nexo-raiden-keycloak

---

## Restart Deployment

kubectl rollout restart \
deployment/nexo-raiden-keycloak \
-n platform

---

## Verify HTTPS

curl -k -I \
https://keycloak.nexo-raiden.local:30443

Expected:

HTTP/2 302

---

## Admin Console

URL:

https://keycloak.nexo-raiden.local:30443
