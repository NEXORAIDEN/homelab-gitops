# ArgoCD Operations

## Verify Applications

kubectl get applications -n argocd

---

## Verify Specific Application

kubectl get application nexo-raiden-keycloak -n argocd

---

## Force Refresh

kubectl annotate application APP_NAME \
  -n argocd \
  argocd.argoproj.io/refresh=hard \
  --overwrite

---

## Force Sync

kubectl patch application APP_NAME \
  -n argocd \
  --type merge \
  -p '{"operation":{"sync":{"prune":true}}}'

---

## View Application Details

kubectl describe application APP_NAME -n argocd

---

## Check ArgoCD Pods

kubectl get pods -n argocd
