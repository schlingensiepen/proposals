---
type: Proposal Rule
description: A service may offer an open registration method; if it does, it answers with token, display string and flag, and only the display string ever reaches a human.
number: F14
binding: must
role: independent
applies_to: [builder, consumer]
---

# F14 · Optional self-registration with a display string

**Applies to:** builder (the offer) · consumer (the flow; delivered by the builder, F11).

**What holds.** A service MAY offer an open registration method alongside the open description methods. The flow in three steps: **(1)** The agent calls it with a free-text **self-introduction** (who, what for). **(2)** The service answers with three things: the **token**, a **display string** (8 characters, lowercase), and a **flag** naming the flavor — **automatic** (pure self-registration: the token is usable immediately) or **by-request** (the token first needs an activation). **(3)** In the "by-request" flavor, the agent shows his user **the display string, never the token** — and the user grants the rights server-side to exactly this display string. In this case the token comes into the world without rights and is switched live afterwards. Protecting the open endpoint against abuse (throttling, expiry of unconfirmed tokens) is the operator's task; the self-introduction is stored with the token and is the basis for deciding about it and for auditing it.

**Why.** Onboarding thereby becomes end-to-end — "URL plus self-introduction" — without a secret ever passing through humans: the display string is the pronounceable, non-secret handle to the token. Whoever reads the token list months later can still tell from the self-introduction who obtained access there and why. And let it be said honestly: **this is not really secure** — it is the deliberate compromise for getting started quickly. The author has tried out more complicated procedures; they made onboarding unusable. The limits are treated by the security considerations in the scope.

**Obvious and rejected.** *A full OAuth build-out* — disproportionate for small services; where an ecosystem prescribes it, it applies anyway. *The token itself as the reference in conversation* — then the secret travels through chat and word of mouth. *Ordering the rights in the same call* — then the anonymous party decides on his own cut; separating creation from activation is the protection.

**How you notice it is violated.** Users dictate tokens through chats or ticket systems — or onboarding fails because nobody with server access happens to be at hand.
