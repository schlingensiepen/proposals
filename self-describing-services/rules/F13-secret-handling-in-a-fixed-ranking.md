---
type: Proposal Rule
description: The usage description prescribes to the consumer the ranking of `where-a-secret-comes-from`; for the consumer, the once-only channel is the context window.
number: F13
binding: must
role: independent
applies_to: [consumer, builder]
imports: [where-a-secret-comes-from]
---

# F13 · Secret handling in a fixed ranking

**Applies to:** consumer (delivered by the builder, F11). Imports `where-a-secret-comes-from`.

**What holds.** The usage description prescribes to the consumer, as one of the consumer duties of F11, that the secret is obtained according to the ranking of `where-a-secret-comes-from`; for the consumer, the once-only channel of that ranking is the context window. The ranking itself is stated by the imported concept; this concept presupposes it and does not repeat it.

**Why.** Chat histories and logs are searchable, copyable stores. The ranking minimizes how long and how often a secret exists in them, and it gives the agent a checkable rule instead of a feeling.

**Considered and rejected.** *"Just put the secret in the chat" as the normal path*: turns the transcript store into a keyring. *Client-side encryption contortions*: the agent cannot keep secrets from himself; false security.

**How you notice it is violated.** A secret turns up in a transcript, a log, or an error message.
