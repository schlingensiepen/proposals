---
type: Proposal Rule
description: The handover runs over a channel outside the chat; recommended is open self-registration with a self-introduction, a display string and a flavor flag, automatic or by-request.
number: F5
binding: must
role: load-bearing
applies_to: [operator, user]
---

# F5 · Handover over a second channel — recommended: self-registration

**Applies to:** Operator · User.

**What holds.** The handover of a token to the consumer runs over a channel outside the chat; what form this second channel takes is settled by the adopter per deployment (see the agent guide). RECOMMENDED as the solution to the setup problem is open **self-registration** in three steps: **(1)** The agent calls it with a free-text self-introduction (who, what for). **(2)** The service answers with a token, a display string, and a flag that names the flavor — **automatic** (immediately usable; only where damage potential is low) or **by-request** (the token is created without rights). **(3)** In the "by-request" flavor, the agent shows its user the display string, never the token — and the user grants the rights server-side to exactly this display string. The self-introduction is stored with the token and is its decision and audit basis. And say so honestly: **this is not really secure** — it is the deliberate compromise in favor of getting started quickly; more complicated procedures have proven unusable in trials.

**Why.** The setup is the moment when secrets want to pass through human hands — every hand is a copy. Self-registration makes the onboarding end-to-end without a secret ever traveling through a human: the human touches only the non-secret handle. Anyone who reads the token list months later can still see from the self-introduction who got access there and why.

**Considered and rejected.** *The chat as the normal path of handover* — turns the transcript store into a keyring; as the declared last resort, F6 regulates it. *A full OAuth build-out for small services* — disproportionate; where an ecosystem prescribes it, it applies anyway (see the scope). *Requesting rights in the registration call* — see F3.

**How you notice it is violated.** Users dictate tokens through chats or ticket systems — or the onboarding fails because nobody with server access happens to be at hand.
