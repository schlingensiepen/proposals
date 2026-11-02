---
type: Proposal Rule
description: The user makes the service available in natural language at a scope, and the agent uses the storage means of his own harness; the mechanics are not standardized.
number: F10
binding: must
role: independent
applies_to: [consumer]
---

# F10 · Making available at scopes — with the harness's own means

**Applies to:** consumer (delivered by the builder, F11).

**What holds.** The user makes the service available with natural language — "make `<url>` available for this session", "install that permanently for me", "make that available for all users" — and for this the agent uses the storage mechanisms of his own harness: session memory, skill catalog, whatever is present. This concept deliberately does not standardize the mechanics.

**Why.** Every harness stores differently, and it knows best itself — a prescribed storage structure would be installation through the back door. The path is proven: on hearing the three scope statements, at least one widespread agent environment fetches and stores the anchor skill correctly in the respectively right place — and it is exactly for this that F8 laid down the active offer with a link on the entry page: the agent finds what the entry page offers him. Because cascade and division leave almost nothing in what is stored, storage stays cheap — resolution happens at the time of use.

**Considered and rejected.** *A standardized registry of remembered services* — the agent has his own mechanisms; we would have invented a structure that every harness solves differently and better itself. *Persistence as a duty* — "for this session" is a legitimate end state; the scope is the user's business.

**How you notice it is violated.** The agent demands format or storage knowledge from the user ("where should I write this?") — or the user has to name the URL anew every time, because nothing was anchored.
