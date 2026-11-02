---
type: Proposal Rule
description: Every capability the outer edge of a deployment advertises to clients is measured from outside with a client that follows the advertisement, and whatever the network in front does not let through is switched off at the edge.
number: F8
binding: must
role: independent
applies_to: [operator, builder]
---

# F8 · What the outer edge advertises, the network in front must let through — checked at deployment, from outside, with a client that follows the advertisement

**Applies to:** operator · builder.

**What holds.** Every capability that the outer edge of a deployment advertises to clients — a further protocol, a further port, an alternative service — is **measured from outside** at go-live and after every change to proxy or network, with a client that **follows** the advertisement, not with one that ignores it. What the network does not let through is not advertised: the advertisement is **switched off** at the outer edge; the client is not lectured. The check belongs to the deployment, because proxy and network stand there and the application does not see its outer edge. Of this, the incident and the measurement are proven; the check has not been built, and the case is still open in the author's delivery. F8 goes beyond the proven implementation at this point (see the evidence).

**Why.** The incident in the author's delivery: the reverse proxy in front advertised, through the `Alt-Svc` header, a further protocol on a UDP port that the network in front of the server did not let through. Clients that followed the advertisement ran into a timeout; the advertisement was valid for thirty days. Every client checked by hand got through — because it did not follow the advertisement — and that led the fault-finding twice in the wrong direction; the explanation a checking agent offered on its own ("the server blocks cloud addresses") was wrong and would have led into a third. The standard for such advertisements allows clients to fall back to the original route but does not require it (see the sources), and a fallback after a timeout is already a failure for an agent with a tight deadline. A service that advertises a capability the network in front does not let through thereby breaks for exactly the clients attentive enough to follow the advertisement — and the break looks like a client problem. This finding is **still open** in the author's delivery (see the evidence); the rule is the generalization drawn from it, and its check is outstanding.

**Considered and rejected.** *Relying on the clients' fallback* — the standard allows it, does not require it, and it comes too late. *Having the application check* — it does not see its outer edge; the advertisement came from the proxy, not from the application. *Switching on all protocols, "it does no harm"* — it does harm exactly when one of them does not get through, and then to the best clients. *Checking only the ports the application uses* — the advertised port was not one of them.

**How you notice it is violated.** External callers fail with a generic connection error while every client checked by hand gets through — and the fault-finding wanders off to certificates, protocol versions and block lists.
