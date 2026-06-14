# Ingress Operations

## Verify Controller

kubectl get pods -n platform

---

## Verify Ingresses

kubectl get ingress -A

---

## Verify IngressClass

kubectl get ingressclass

---

## View Controller Logs

kubectl logs \
  -n platform \
  deploy/ingress-nginx-controller

---

## Verify Routes

curl -I http://keycloak.nexo-raiden.local:30080

curl -k -I \
https://keycloak.nexo-raiden.local:30443

---

## Restart Controller

kubectl rollout restart deployment \
  ingress-nginx-controller \
  -n platform
