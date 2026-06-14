# PostgreSQL Troubleshooting

## Pod CrashLoopBackOff

Possible Causes:

- Missing secret
- Incorrect credentials
- Missing PVC

Verification:

kubectl describe pod

kubectl logs

---

## Database Connection Failure

Symptoms:

Application cannot connect.

Verification:

kubectl get service -n data

kubectl get endpoints -n data

---

## Secret Missing

Symptoms:

Startup failure.

Verification:

kubectl get secret -n data

kubectl get externalsecret -n data

---

## PVC Pending

Symptoms:

Pod not starting.

Verification:

kubectl get pvc -n data

kubectl describe pvc
