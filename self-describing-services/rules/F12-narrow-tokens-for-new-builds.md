---
type: Proposal Rule
description: A new build should cut access as narrow capabilities, one token, one capability, one place, issued server-side and shown once.
number: F12
binding: should
role: independent
applies_to: [builder]
---

# F12 · Narrow tokens as the recommendation for new builds

**Applies to:** builder.

**What holds.** A newly built service SHOULD cut access rights as narrow capabilities: one token embodies exactly one capability at exactly one place; whoever needs two targets gets two tokens. **The base path of issuance is the operator:** he generates the token server-side and displays it exactly once. Deviations from the token model are permissible with good reasons — an existing API brings along whatever it has (see "Access" in the concept).

**Why.** In the agent context, the chat history is a real leak channel. Narrow tokens make every single leak surgically small — revocation and rotation hit exactly one purpose, not the whole service. The deeper justification is carried by the sister concept `capability-tokens-for-agents`; this rule summarizes what is needed here.

**Obvious and rejected.** *An admin token "just to be safe"* — makes every leak total. *A scope parameter on the call, checked against the token* — the token itself is meant to be the scope; a selectable parameter is authorization code that can be wrong.

**How you notice it is violated.** Revoking a single token shuts down more than the one purpose it was issued for.
