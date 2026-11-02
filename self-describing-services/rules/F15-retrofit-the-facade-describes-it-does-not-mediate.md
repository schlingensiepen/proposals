---
type: Proposal Rule
description: For an existing API, a description service of its own may deliver the cascade; it renders the real target and never touches the data traffic.
number: F15
binding: must
role: independent
applies_to: [façade operator]
---

# F15 · Retrofit: the façade describes, it does not mediate

**Applies to:** façade operator (retrofitter).

**What holds.** For an existing API, the cascade MAY be delivered by a description service of its own that does not touch the data traffic. In doing so the façade delivers the **complete self-description according to this building principle** — entry page, usage description, pumped-up interface description (F6), anchor skill — except that the description says explicitly: "execute the operations described here directly against `<original endpoint>`". In the live templating (F5), the **real target** is rendered for this, never the façade itself. The façade names the version, or the state, of the target API that it describes; the way to check against drift of the target API is deliberately left open (see "Not yet settled" in the scope). If you want to serve several instances of the same service software, the recommended build form is the **generic** one: a description service that is given the target instance in the URL — `https://wiki-facade.example/refer-to/wiki.example.org` describes the same software, rendered onto exactly this instance. The example is deliberately a wiki: precisely there one often has access to several instances that all follow the same API.

**Why.** This makes someone else's API immediately agent-usable without the description service stepping into the data path — no additional latency, no responsibility for payload data, and secrets continue to flow only between consumer and original service.

**Considered and rejected.** *A full proxy* — takes on data-path responsibility, costs latency, and routes other people's secrets through third parties. *Distributing skill files in the repository* — it works (see the evidence: a wiki with 140,000 nodes was built that way), but it is the distributed copy with all the consequences from F1.

**How you notice it is violated.** The façade describes operations that the target service, after an update, no longer offers or offers differently — and no consumer learns of it until a call fails.
