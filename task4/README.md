# Task 4 — Reconnaissance & Penetration Testing

## Scope and Methodology

Task 4 was divided into:

1. Passive reconnaissance against `dodopayments.tech`.
2. Active security testing only against the authorized local vulnerable application.

No exploitation, fuzzing, brute forcing, or intrusive scanning was performed against Dodo Payments production infrastructure.

---

# Part A — Passive Reconnaissance

## Certificate Transparency

Certificate Transparency data from crt.sh was queried for:

`%.dodopayments.tech`

112 unique hostnames were identified.

Examples included:

- app.dodopayments.tech
- checkout.dodopayments.tech
- keycloak.dodopayments.tech
- sonarqube.dodopayments.tech
- argocd.dodopayments.tech
- grafana.dodopayments.tech
- kafka.dodopayments.tech
- sentry.dodopayments.tech
- wordpress.dodopayments.tech

The complete result is stored in:

`recon/crt-subdomains.txt`

## DNS Observations

Selected DNS records were inspected.

Observations:

- `dodopayments.tech` and `app.dodopayments.tech` resolved to Cloudflare infrastructure.
- `checkout.dodopayments.tech` used a Vercel DNS target.
- `keycloak.dodopayments.tech` and `sonarqube.dodopayments.tech` resolved publicly.
- Several CT-discovered names including `argocd`, `grafana`, and `wordpress` did not resolve during testing.

Certificate Transparency records may represent historical infrastructure and therefore do not prove that every discovered hostname remains active.

## HTTP / Technology Observations

Lightweight HTTP header inspection was performed against selected public endpoints.

### app.dodopayments.tech

Returned HTTP 200.

Observed:

- Cloudflare-facing infrastructure
- Next.js static resource indicators
- HSTS
- Content Security Policy
- X-Content-Type-Options
- X-Frame-Options
- Referrer-Policy

### checkout.dodopayments.tech

Returned HTTP 404.

Observed:

- Vercel infrastructure
- Next.js indicators
- HSTS
- Permissions-Policy
- CSP containing `frame-ancestors *`

The permissive frame-ancestor configuration is recorded as an attack-surface observation and was not actively exploited.

### keycloak.dodopayments.tech

Returned HTTP 302 redirecting to `/admin/`.

This indicates an externally reachable identity/administrative interface. Public exposure of administrative surfaces increases attack surface and should be protected using strong authentication, access controls, monitoring, and where appropriate network restrictions.

### sonarqube.dodopayments.tech

Returned HTTP 200.

The externally reachable SonarQube service represents development/security infrastructure that should be carefully access-controlled and monitored.

## TLS Observation

TLS certificate inspection of `app.dodopayments.tech` showed:

- Subject: `CN=dodopayments.tech`
- Issuer: Google Trust Services WE1
- Valid from: 24 June 2026
- Valid until: 22 September 2026

The certificate was valid during assessment testing.

## Attack Surface Summary

Passive reconnaissance identified a comparatively broad set of application, development, observability, authentication, and infrastructure-related hostnames.

Higher-value surfaces visible through public metadata included identity systems, source/security analysis tooling, observability platforms, and historical infrastructure interfaces.

These observations do not by themselves demonstrate vulnerabilities. They identify assets that warrant strong access control, patch management, monitoring, and exposure review.

---

# Part B — Authorized Local Penetration Test

Active testing was restricted to the intentionally vulnerable application supplied with the assessment.

## Finding 1 — Unauthenticated Payment Card Data Disclosure

**Severity:** High

**CVSS:** 8.2 (estimated)

**OWASP Category:** Broken Access Control / Cryptographic Failures

### Description

The `/transactions` endpoint does not require authentication or authorization.

An unauthenticated client can retrieve transaction records containing complete Primary Account Numbers (PANs).

### Proof of Concept

```bash
curl http://127.0.0.1:8085/transactions
