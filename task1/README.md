# Task 1 — Kubernetes & Container Security Hardening

This task implements security hardening for the ledger API and its Kubernetes deployment.

## Implemented Controls

- Hardened container image and Dockerfile
- Non-root container execution
- Kubernetes securityContext controls
- Resource requests and limits
- RBAC with least-privilege permissions
- Kubernetes Secret management using Sealed Secrets
- Kyverno admission policies
- Container security validation
- Image vulnerability scanning
- Secret scanning

## Relevant Files

- `../app/Dockerfile`
- `../deploy/deployment.yaml`
- `../deploy/rbac.yaml`
- `../deploy/sealed-secret.yaml`
- `../deploy/kyverno-security.yaml`
- `../kyverno-config.yaml`
- `../.gitleaks.toml`

## Evidence

Execution and validation screenshots are available under:

`../Screenshots/task1/`
