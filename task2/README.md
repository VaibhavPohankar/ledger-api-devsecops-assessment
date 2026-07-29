# Task 2 — Secure CI/CD & GitOps

This task implements a security-focused CI/CD and GitOps workflow for the ledger API.

## Implemented Controls

- Automated CI/CD workflow
- Secret scanning with Gitleaks
- Container vulnerability scanning with Trivy
- Secure container image build and publishing
- Container image signing with Cosign
- Build provenance/attestation
- Kubernetes image verification using Kyverno
- GitOps deployment using Argo CD
- Security gates to prevent insecure artifacts from progressing through the pipeline

## Relevant Files

- `../.github/workflows/`
- `../.gitleaks.toml`
- `../app/Dockerfile`
- `../deploy/argocd-application.yaml`
- `../deploy/kyverno-verify-images.yaml`
- `../security/cosign/`

## Evidence

Execution and validation screenshots are available under:

`../Screenshots/task2/`
