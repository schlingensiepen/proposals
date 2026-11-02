---
type: Proposal Rule
description: Recommended: every token gets a short, pronounceable, non-secret display string, and humans and agents speak about tokens, grant and revoke only by that handle.
number: F11
binding: should
role: load-bearing
applies_to: [operator, agent, user]
---

# F11 · Way of speaking: the display string

**Applies to:** Operator · Agent · User.

**What holds.** RECOMMENDED: every token receives, at creation, a short, pronounceable, **non-secret** handle — the display string (eight characters, lowercase). Humans and agents speak about tokens exclusively by display string; granting rights, revocation, and inquiries address the handle, never the secret.

**Why.** Tokens *have* to be spoken about — granted, revoked, assigned, asked after — and every conversation that needs the secret itself moves the secret. The handle makes speaking safe: dictation-proof on the telephone, readable in the token list, unambiguous in the granting of rights. The short ID from F2 does the technical work (logs); the display string does the human work.

**Considered and rejected.** *The token itself as the reference in conversation* — then the secret travels through chat and hallway conversation. *Internal IDs as the handle* — not pronounceable, not dictation-proof; what people cannot use comfortably, they circumvent.

**How you notice it is violated.** Token fragments turn up in chats or tickets because there was no other name for the thing.
