# Ingress NGINX

## Purpose

HTTP entry point for the platform.

## Why We Need It

Routes traffic to services.

## Namespace

platform

## Files

apps/ingress-nginx.yaml

## Ports

30080 HTTP

30443 HTTPS

## Verification

kubectl get ingressclass

kubectl get ingress -A

## Example

keycloak.nexo-raiden.local
