# Task 3 — Service Mesh & Zero-Trust Networking

## Overview

Istio was deployed to provide service-to-service identity, mutual TLS, and authorization for workloads in the `payments` namespace.

The security model uses multiple layers:

1. Istio STRICT mTLS for encrypted and authenticated service communication.
2. Istio AuthorizationPolicy for workload identity-based access control.
3. Kubernetes NetworkPolicy for network-level segmentation and defence-in-depth.

## Istio Service Mesh

The `payments` namespace uses automatic Istio sidecar injection. The `ledger-api` and `reporting` workloads run with Envoy sidecars.

## STRICT mTLS

A `PeerAuthentication` named `payments-strict-mtls` enforces STRICT mTLS in the namespace.

This prevents workloads from communicating with meshed services using unauthenticated plaintext connections.

## Identity-Based Authorization

An Istio `AuthorizationPolicy` named `ledger-api-allow-reporting` restricts access to `ledger-api`.

Access is based on workload identity/service account rather than source IP.

Testing demonstrated:

- Authorized `reporting` identity -> HTTP 200
- Unauthorized identity -> HTTP 403

This ensures network location alone does not grant access.

## Workload Certificates and Trust

Istio provides workload identities using X.509 certificates.

When an Envoy sidecar starts, the Istio agent requests a certificate for the workload identity from Istio's certificate authority. The identity is derived from the Kubernetes service account and represented using a SPIFFE-style identity.

Certificates are short-lived and automatically renewed by Istio before expiration, avoiding manually managed long-lived service certificates.

The Istio CA establishes the trust root for workloads in the mesh. Envoy proxies use the trusted root CA to validate certificates presented by peer workloads during mutual TLS authentication.

Certificate status was verified using:

`istioctl proxy-config secret <ledger-api-pod> -n payments`

The output confirmed an ACTIVE and valid workload certificate chain and an ACTIVE ROOTCA.

## Kubernetes NetworkPolicy

NetworkPolicies provide an additional network-level security boundary underneath the service mesh.

A namespace-wide default-deny policy blocks traffic unless explicitly permitted.

Explicit policies allow:

- DNS access
- Required Istio control-plane communication
- `reporting` -> `ledger-api` on TCP/8080

Testing demonstrated:

- Authorized reporting traffic -> `status ok`
- Unauthorized workload -> HTTP 503 / Envoy upstream connection failure

## Defence in Depth

Istio and NetworkPolicy address different security layers.

**Istio AuthorizationPolicy** understands authenticated workload identity and can determine which service account is allowed to communicate with an application.

**Kubernetes NetworkPolicy** operates at the network layer and restricts which pods may establish network connections.

Therefore, NetworkPolicy reduces network reachability while Istio provides cryptographic workload authentication and identity-aware authorization.

If one control is misconfigured or bypassed, the other still provides an additional security boundary.
