# PostgreSQL Operations

## Verify Pod

kubectl get pods -n data

---

## Verify PVC

kubectl get pvc -n data

---

## Verify Secrets

kubectl get secret -n data

---

## Verify External Secret

kubectl get externalsecret -n data

---

## View Logs

kubectl logs -n data deploy/nexo-raiden-postgresql

---

## Open Database Shell

kubectl exec -it \
  deploy/nexo-raiden-postgresql \
  -n data -- bash

---

## Connect to PostgreSQL

psql -U postgres

---

## Restart

kubectl rollout restart \
deployment/nexo-raiden-postgresql \
-n data
