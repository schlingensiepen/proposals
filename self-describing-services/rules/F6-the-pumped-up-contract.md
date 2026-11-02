---
type: Proposal Rule
description: The generated interface description is treated as a product and enriched, every field and operation described with a real example, enforced as a gate in the build.
number: F6
binding: must
role: independent
applies_to: [builder]
---

# F6 · The pumped-up contract

**Applies to:** builder.

**What holds.** The generated interface description is treated as a product and editorially enriched — with OpenAPI concretely: **(1)** every field carries a description that does not repeat the name but names semantics, units, limits, and typical errors, plus a realistic example; **(2)** every endpoint carries a short and a long description with the non-obvious rules — this is where the behavior that belongs to the operation moves (see "Two kinds of description" in the concept); **(3)** the preamble contains the workflow in short form in a few lines; **(4)** errors are documented like successes, every error type with an example response; **(5)** all of this is enforced as a check ("no operation, no field without a description and an example" as a gate in the build). Other interface dialects bring their own contract mechanisms — GraphQL its introspection with description fields, JSON-RPC the OpenRPC document, gRPC its reflection — and are enriched analogously; appendices per technology are envisaged, in case it turns out that the analogy has to be spelled out in detail.

**Why.** The contract exists anyway (see "The starting point" in the concept) — enriched, it turns from a formality into complete documentation of the mechanics that cannot go stale, because it is generated from the same code as the behavior of the service. And the curation *inside* the carrier is what distinguishes it from a curated side list: it is delivered along with the carrier and does not age separately.

**Considered and rejected.** *Deliver the raw schema and be done with it* — completeness without weighting; the consumer does not learn what is important, what belongs together, what has which consequences. *A separate curated operation list next to the schema* — the author has built this precursor himself; it works and proves the value of curation, but it produces three artifacts aging side by side (see the evidence). *Mandatory descriptions as an intention rather than as a gate* — erodes with the first hurried release.

**How you notice it is violated.** The prose instructions grow while the interface's description fields stay empty — or a consumer needs a second document besides the contract in order to build a single call correctly.
