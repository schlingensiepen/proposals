---
type: Proposal Rule
description: A token embodies exactly one ability at exactly one place; whoever needs two targets or two abilities gets two tokens; a legacy API keeps what it has, and its inherited secret still gets the full handling rules.
number: F1
binding: must
role: load-bearing
applies_to: [operator]
---

# F1 · One token, one ability, one place

**Applies to:** Operator.

**What holds.** A newly built service SHOULD cut access rights as narrow capabilities: one token embodies exactly one ability at exactly one place; whoever needs two targets or two abilities gets two tokens. Deviations are permissible with good reasons — a legacy API brings what it has; for its inherited secret, the handling rules F5–F11 then apply unchanged.

**Why.** In the agent context, the chat history is a real leak channel. Narrow tokens make every single leak surgically small: revocation and rotation hit exactly one purpose, not the whole service — and the user can grant rights at the granularity in which they think ("may report traffic jams") instead of the granularity an admin model imposes.

**Obvious and rejected.** *One admin token "to be safe"* — makes every leak total; today's convenience is tomorrow's total loss. *One collective token per user across all of their purposes* — couples the unconnected: a leak at the side purpose opens the main purpose.

**How you notice it is violated.** Revoking a single token silences more than the one purpose it was issued for.
