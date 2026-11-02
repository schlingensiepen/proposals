---
type: Proposal Rule
description: The document is retrievable without any credential and therefore contains exactly two things, form and hydration logic; everything of substance arrives through the API, and every request yields the same bytes.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · The file is delivered unauthenticated — and therefore carries no data

**Applies to:** builder.

**What holds.** The document is retrievable without any credential. For precisely this reason it contains **exactly two things: form and hydration logic** — the empty structure with its labels, and the code that fills it at runtime. **Everything of substance arrives through the API**: no names, no lists, no prefilled values, no keys, not a single record. Every request yields the same bytes.

**The dividing line is not "sounds domain-specific" but "changes with the data".** A label such as "storage location" is form, although it comes from the domain vocabulary — it reads the same in every request. A concrete storage location is content. The test in case of doubt: *would something else stand here for a different dataset?* If so, it does not belong in the document but in a response.

**Why.** This is not a sacrifice but a condition. The application is what **obtains** a credential — you cannot demand one for loading it without going in a circle. Since everyone gets it, everything in it is published. The gain: **the frontend stops being a trust boundary.** There is no check that must be right at delivery time and no request that shows more than another. What needs protecting is behind the API and is checked there — in one place instead of two.

**Considered and rejected.** *Fill the first screen server-side so it is not empty* — that turns delivery back into an authorization question, and the check exists in two places. *Put the file behind a simple access barrier* — it would have to apply before the invitation flow, hence before everything that creates a credential; that is the circle above. *Embed "just" the configuration* (API address, application name) — as soon as something is embedded, the question becomes what else, and the boundary shifts with every new request for an exception. Configuration belongs in a separate, equally public file — then the rule is unambiguous instead of negotiable.

**How you notice it is violated.** Two requests for the document yield different bytes.
