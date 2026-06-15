# cert-manager Troubleshooting

## Application Missing

Symptoms:

ArgoCD:

OutOfSync
Missing

Cause:

Helm deployment failure.

Verification:

kubectl get applications -n argocd

---

## ClusterIssuer Missing

Symptoms:

kubectl get clusterissuer

returns:

server doesn't have a resource type

Cause:

CRDs not installed.

Verification:

kubectl get crd | grep cert-manager

---

## Certificate Not Created

Symptoms:

No Certificate resource.

Cause:

Ingress annotation missing.

Verification:

kubectl describe ingress

Check:

cert-manager.io/cluster-issuer

---

## TLS Secret Missing

Symptoms:

Secret not found

Cause:

Certificate issuance failure.

Verification:

kubectl get certificate -A

---

## HTTPS Not Working

Symptoms:

Browser TLS error

Verification:

kubectl get secret keycloak-nexo-raiden-local-tls -n platform

Expected:

TYPE:

kubernetes.io/tls
