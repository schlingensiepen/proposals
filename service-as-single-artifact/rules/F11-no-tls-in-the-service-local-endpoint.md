---
type: Proposal Rule
description: The service listens on a local endpoint; public names, certificates and TLS are the business of what the deployment places in front of it, usually a ready-made reverse proxy.
number: F11
binding: must
role: independent
applies_to: [builder, operator]
---

# F11 · No TLS in the service, local endpoint

**Applies to:** builder · operator.

**What holds.** The service listens on a local endpoint; public names, certificates and TLS are served by a reverse proxy in front of it. The process form — systemd unit or container runtime — is freely selectable; the artifact stays the same.

**Why.** Obtaining and renewing certificates is solved in ready-made proxy containers (Caddy, Traefik, nginx with an ACME companion); a service that does it itself carries a load it need not carry and ties renewals to its restarts. And a service that listens only locally has no external surface at all without a proxy — the exposure is a deliberate act, not a default state. What is in front of the local endpoint, a proxy, a tunnel or nothing, is decided by the deployment, which bears the price if it is nothing.

**Considered and rejected.** *TLS in the service* — a certificate client of its own, renewal logic of its own, restarts at every certificate change. *Listening directly on all interfaces* — the exposure becomes an oversight instead of a decision.

**How you notice it is violated.** The service has to be restarted because a certificate was renewed.
