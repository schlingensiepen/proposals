---
type: Proposal Rule
description: The configuration says where a secret comes from and never contains it; the ranking of sources is the sister concept's business, and no secret ever appears in the check's output.
number: F3
binding: must
role: load-bearing
applies_to: [builder, operator]
imports: [where-a-secret-comes-from]
---

# F3 · Secrets are referenced, not contained

**What holds.** The configuration says where a secret comes from; it
does not contain it. Where it comes from, in which ranking, and what
holds when it is set in several places is the business of the sister
concept `where-a-secret-comes-from`; this concept presupposes it. A
secret never appears in the check's output, not even truncated.

**Why.** A configuration file gets versioned, copied, pasted into
tickets. What is in it is soon everywhere.

**Obvious and rejected.** *The secret in the file "with tight
permissions"*: the permissions do not survive the first copy. *Showing
the secret masked in the check's output*: the masking is switched off at
the first incident.

**How you notice it is violated.** A secret sits in a file that is under
version control, or in the output of the check.
