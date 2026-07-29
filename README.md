# Dodo Payments — Security & DevOps Technical Assessment

This repository contains my implementation of the Dodo Payments Security & DevOps Engineer technical assessment.

The assessment covers Kubernetes workload hardening, secure software delivery and supply-chain security, zero-trust networking, and security reconnaissance / penetration testing.

## Tasks

### [Task 1 — Deploy & Harden the Workload](task1/README.md)

Implemented Kubernetes workload security controls including:

- Non-root containers and hardened security contexts
- Resource requests/limits and health probes
- Least-privilege ServiceAccount and RBAC
- Sealed Secrets for secret management
- Kyverno admission policies and security guardrails

### [Task 2 — Secure CI/CD & Supply Chain](task2/README.md)

Implemented a security-focused delivery pipeline including:

- GitHub Actions CI/CD
- Semgrep, Trivy and Gitleaks security gates
- Container image scanning
- Cosign image signing
- Build provenance/attestation
- Kyverno image verification
- Argo CD GitOps deployment

### [Task 3 — Zero-Trust Networking](task3/README.md)

Implemented Kubernetes and Istio network security including:

- Default-deny NetworkPolicies
- Explicit service communication rules
- Istio STRICT mTLS
- Identity-based AuthorizationPolicy
- Network segmentation validation

### [Task 4 — Reconnaissance & Penetration Testing](task4/README.md)

Performed:

- Passive reconnaissance of the permitted public attack surface
- DNS and certificate-transparency enumeration
- HTTP/TLS posture review
- Focused penetration testing against the authorized local vulnerable application
- Documented findings with PoCs, impact, CVSS and remediation

## Evidence

Screenshots demonstrating deployments, security controls, pipeline execution, policy enforcement, reconnaissance and penetration-testing PoCs are available in:

[**Screenshots/**](Screenshots/)

## Repository Structure

- `app/` — ledger-api application and container configuration
- `deploy/` — Kubernetes, Kyverno, Istio and GitOps manifests
- `task1/` — Task 1 documentation
- `task2/` — Task 2 documentation
- `task3/` — Task 3 documentation and NetworkPolicies
- `task4/` — Reconnaissance and penetration-test report
- `Screenshots/` — assessment evidence
- `.github/workflows/` — CI/CD and security automation
- `security/` — supply-chain security artifacts
