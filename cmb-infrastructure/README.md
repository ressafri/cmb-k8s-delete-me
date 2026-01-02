# CMB Infrastructure

This directory contains shared infrastructure resources managed by ArgoCD.

## Purpose

Shared resources like namespaces should be managed by a single ArgoCD application to avoid `SharedResourceWarning` conflicts.

## Resources

- `namespace.yaml` - Creates the `cmb-production` namespace used by all applications

## Why This Exists

Previously, both `cmb-portfolio` and `cmb-feed` had their own `namespace.yaml` files, causing ArgoCD to warn about shared resource management. By centralizing the namespace in this infrastructure app, we ensure:

- Clear ownership of shared resources
- No conflicts between applications
- Better separation of concerns
- Easier management of shared infrastructure

## Deployment

This is deployed via the `cmb-infrastructure` ArgoCD application defined in `cmb-apps/infrastructure.yaml`.

