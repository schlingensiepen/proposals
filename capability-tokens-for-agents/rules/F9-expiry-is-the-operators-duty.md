---
type: Proposal Rule
description: Never-activated and permanently unused tokens expire automatically and open issuance paths are throttled; the concrete deadlines and thresholds are set and recorded by the adopter.
number: F9
binding: must
role: load-bearing
applies_to: [operator]
---

# F9 · Expiry is the operator's duty — the values are set by the adopter

**Applies to:** Operator.

**What holds.** Tokens created without rights that are never activated, and tokens that remain permanently unused, expire automatically; open issuance paths (self-registration) are throttled. The concrete deadlines and thresholds are deliberately not standardized by this concept — they are operational values that only the respective operator knows, and hence an assignment to the adopter, who records them (see the agent guide).

**Why.** Stockpiling is the quiet opposite of the narrow cut: every forgotten token is an ability that nobody misses any more and everybody can still use. An open issuance path without throttling and expiry accumulates exactly such tokens.

**Considered and rejected.** *Standardizing uniform deadlines in the concept* — comparable and checkable, but wrong: the same deadline is generous for the hobby service and negligent for the company. *No expiry ("nobody is going to abuse it")* — hope as security architecture.

**How you notice it is violated.** The token list grows monotonically, and nobody can say what half the entries belong to.
