# ADR-0002: External Secrets Version Pinning

## Status

Accepted

## Decision

Use External Secrets Helm chart version `0.16.2`.

## Context

Chart version `2.6.0` caused CRD annotation-size failures and missing `SecretStore` / `ClusterSecretStore` CRDs on the Raspberry Pi K3s environment.

## Decision Drivers

- Stable CRD installation
- ARM64 compatibility
- GitOps repeatability
- Reduced bootstrap complexity

## Consequences

- External Secrets runs reliably on the Pi.
- Future upgrades must be tested in a branch before changing the pinned version.
