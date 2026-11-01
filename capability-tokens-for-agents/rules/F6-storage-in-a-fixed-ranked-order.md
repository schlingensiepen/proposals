---
type: Proposal Rule
description: The agent obtains and keeps a secret by the ranking of where-a-secret-comes-from; for the agent, the once-only channel is the context window, named once, persisted immediately, never repeated.
number: F6
binding: must
role: load-bearing
applies_to: [agent]
imports: [where-a-secret-comes-from]
---

# F6 · Storage in a fixed ranked order

**Applies to:** Agent (delivered by the operator, F12). Imports `where-a-secret-comes-from`.

**What holds.** The agent obtains and keeps the secret by the ranking of `where-a-secret-comes-from`: the environment (under the canonical name the service proposes), then a pass-through file, last the once-only channel. For the agent, the once-only channel is the context window: named once, persisted immediately, never repeated. The ranking applies to every secret, whether token or inherited API key; this concept presupposes it and does not repeat it.

**Why.** Chat histories and logs are searchable, copyable stores. The ranking minimizes how long and how often a secret exists in them, and it gives the agent a checkable rule instead of a feeling. The canonical environment-variable name incidentally solves the re-finding problem across sessions: the service itself says where its secret usually lives. The same ranking holds for the service that reads and for the agent that obtains; that is why it stands in one place and is imported here.

**Obvious and rejected.** *"Just put the secret in the chat" as the normal path* — the last resort is declared an exception, not permitted as a habit. *Client-side encryption contortions* — the agent cannot have secrets from itself; security theater.

**How you notice it is violated.** A secret turns up in a transcript, a log, or an error message.
