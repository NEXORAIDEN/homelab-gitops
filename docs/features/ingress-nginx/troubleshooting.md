# Ingress Troubleshooting

## Application OutOfSync

Cause:

IngressClass not allowed by AppProject.

Error:

resource networking.k8s.io:IngressClass is not permitted

Resolution:

Add:

IngressClass

to AppProject whitelist.

---

## 404 Not Found

Cause:

Ingress missing.

Verification:

kubectl get ingress -A

---

## DNS_PROBE_FINISHED_NXDOMAIN

Cause:

hosts file missing.

Resolution:

Add:

192.168.178.200 keycloak.nexo-raiden.local

---

## 503 Service Unavailable

Cause:

Backend service missing.

Verification:

kubectl describe ingress

Check backend service exists.

---

## TLS Secret Missing

Cause:

cert-manager not issuing certificate.

Verification:

kubectl get certificate -A

kubectl get clusterissuer
