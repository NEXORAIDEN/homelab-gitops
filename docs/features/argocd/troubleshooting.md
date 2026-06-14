# ArgoCD Troubleshooting

## Application OutOfSync

Symptoms:

OutOfSync

Cause:

Git differs from cluster.

Resolution:

Refresh and sync application.

---

## Resource Not Permitted

Example:

IngressClass not permitted

Cause:

AppProject missing clusterResourceWhitelist

Resolution:

Add required resource.

---

## Repository Not Allowed

Cause:

sourceRepos missing repository.

Resolution:

Add repository to AppProject.

---

## Namespace Not Allowed

Cause:

Destination namespace missing.

Resolution:

Add destination namespace.

---

## Application Missing

Cause:

Manifest path incorrect.

Resolution:

Verify path and repository.
