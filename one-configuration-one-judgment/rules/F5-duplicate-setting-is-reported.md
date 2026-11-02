---
type: Proposal Rule
description: When the same setting is given at several ranks, the higher rank wins and the check names the lower one with its place; applies with per-value precedence only.
number: F5
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F5 · Duplicate setting is reported

Applies with per-value precedence only
([W1 b](../choices/W1-precedence-file-or-value.md)).

**What holds.** If the same setting is given at several ranks, the
higher rank wins, and the check names the lower one with its place.

**Why.** A precedence makes overriding easy and cleaning up invisible.
Anyone who sets something at the top sees the effect at once; anyone
who forgets something at the bottom notices only when the top goes
away: on a migration, on recreating the deployment from a template, on
a typo. Then a value is in effect that nobody knows about any more.

**Considered and rejected.** *Silently taking the winner*: this is how most
services are built, and it is exactly how the incident behind this rule
happened.

**How you notice it is violated.** After removing a variable, a value is
in effect that nobody believes they set.
