---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, scope, version.
* [Concept](./concept.md) - The problem, the concept in prose, the six-step procedure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept.

* [F1 The ranking is fixed and named](./rules/F1-the-ranking-is-fixed-and-named.md) - Environment, then a pass-through file, then, only where W1 allows it, the once-only channel; further sources per W3.
* [F2 Environment first, under a canonical name](./rules/F2-environment-first-under-a-canonical-name.md) - The service proposes the variable name.
* [F3 The file only passes through](./rules/F3-the-file-only-passes-through.md) - Tight permissions, content flows straight into the call, appears nowhere.
* [F4 The higher rank wins and never hides](./rules/F4-the-higher-rank-wins-and-never-hides.md) - Lower ranks that are also set are reported at start, with their place and how to clear them.
* [F5 Replacing is two moves](./rules/F5-replacing-is-two-moves.md) - Replace at the top, clear at the bottom; a way to do it without a shell for files the service wrote.
* [F6 Never in any output](./rules/F6-never-in-any-output.md) - No secret in output, logs, error messages, transcripts or addresses; the only exception is the once-only channel under W1 (b).

# Choices

More than one legitimate answer; decided per secret, in practice per group, by whoever adopts the concept, and recorded.

* [W1 No secret set](./choices/W1-no-secret-set.md) - Refuse to start, or generate and show exactly once.
* [W2 Set but empty or unusable](./choices/W2-set-but-empty-or-unusable.md) - Abort, or warn and continue.
* [W3 Further sources](./choices/W3-further-sources.md) - A secret manager as a rank of its own, or as a feeder to the first two ranks.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, where the answer flips, what it does not solve, what is not yet proven.
* [Evidence](./evidence.md) - Where it has held: origin in the series, the latent access incident, the named exception.
* [Sources](./sources.md) - What this proposal builds on: related proposals, a tension, external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent adopting this concept, including the mandatory step of deciding the choices with the user.
* [Extensions](./extensions.md) - Proposals that extend this one. None yet.

# History

* [Update log](./log.md) - Dated changes to this bundle.
