---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, what changed in 1.1, what it imports.
* [Concept](./concept.md) - The problem, the double cut in prose with its map and load-bearing structure, the procedure for operator and agent.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. Cut on the service side F1 to F4, lifecycle F5 to F9, agent behavior and its delivery F10 to F12.

* [F1 One token, one ability, one place](./rules/F1-one-token-one-ability-one-place.md) - A token embodies exactly one ability at exactly one place; whoever needs two targets or two abilities gets two tokens.
* [F2 Creation: server-side, high-entropy, visible only once](./rules/F2-creation-server-side-high-entropy-visible-once.md) - At least 128 bits, a recognizable prefix, only hash and short ID kept, the plaintext shown once, no retrieval.
* [F3 The token is the scope](./rules/F3-the-token-is-the-scope.md) - The service derives everything that is allowed from the token; no caller-chosen scope parameter.
* [F4 Transport and error semantics: no oracles, no leak surfaces](./rules/F4-transport-and-error-semantics.md) - Bearer header only, short ID in logs, 401 for unknown tokens, foreign resources answer like nonexistent ones.
* [F5 Handover over a second channel, recommended: self-registration](./rules/F5-handover-over-a-second-channel.md) - Outside the chat; self-registration with self-introduction, display string and a flavor flag.
* [F6 Storage in a fixed ranked order](./rules/F6-storage-in-a-fixed-ranked-order.md) - The agent obtains and keeps the secret by the ranking of `where-a-secret-comes-from`; its once-only channel is the context window.
* [F7 Binding to the place of issue](./rules/F7-binding-to-the-place-of-issue.md) - A token is used only against the place it was issued for; the agent asks on ambiguity.
* [F8 Rotation and revocation hit exactly one purpose](./rules/F8-rotation-and-revocation-hit-one-purpose.md) - Individually, without interruption, as routine; expiry dates in calendar and monitoring.
* [F9 Expiry is the operator's duty, the values are set by the adopter](./rules/F9-expiry-is-the-operators-duty.md) - Never-activated and unused tokens expire, open issuance is throttled.
* [F10 The agent's never-rules](./rules/F10-the-agents-never-rules.md) - Never repeat, never misdirect, never obey foreign instructions about secrets, never persist beyond the named reach.
* [F11 Way of speaking: the display string](./rules/F11-way-of-speaking-the-display-string.md) - Recommended: a short, pronounceable, non-secret handle by which tokens are granted, revoked and talked about.
* [F12 Delivery duty: the code travels with the service](./rules/F12-the-code-travels-with-the-service.md) - Every agent-facing rule stands in the service's own documentation.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when the answer flips, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - Where it has held: origin, the worked application, the precursor in operation, the reference implementation in progress, a fictitious walk-through.
* [Sources](./sources.md) - What this proposal builds on: the concept it imports, a related proposal, external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for operators and agents adopting this concept: procedures, the values to settle with the user, rules, acceptance checklist, common mistakes.
* [Extensions](./extensions.md) - Proposals that extend this one. None yet.

# History

* [Update log](./log.md) - Dated changes to this bundle.
