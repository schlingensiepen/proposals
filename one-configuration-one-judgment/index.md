---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, version, what it imports.
* [Concept](./concept.md) - The problem, the concept in prose, the five-step procedure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept.

* [F1 The judgment is the start](./rules/F1-the-judgment-is-the-start.md) - Whatever checks the configuration is the code that starts on it; the check shows the effective values, defaults included.
* [F2 The check names the source](./rules/F2-the-check-names-the-source.md) - Every command says which file it used; with per-value precedence, every value says where it came from.
* [F3 Secrets are referenced, not contained](./rules/F3-secrets-are-referenced-not-contained.md) - The configuration says where a secret comes from and never contains or prints it; the ranking lives in `where-a-secret-comes-from`.
* [F4 Check without binding](./rules/F4-check-without-binding.md) - The check binds no port and writes no state; it can run beside the running service.
* [F5 Duplicate setting is reported](./rules/F5-duplicate-setting-is-reported.md) - When the same setting is given at several ranks, the higher wins and the check names the lower. Per-value precedence only.

# Choices

More than one legitimate answer; decided per setting by whoever adopts the concept, and recorded.

* [W1 Precedence: file or value](./choices/W1-precedence-file-or-value.md) - Does precedence pick the file, or does it apply to each value?
* [W2 The empty value](./choices/W2-the-empty-value.md) - Is a set-but-empty value the default, or an error?

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, where the answer flips, what is not yet proven.
* [Evidence](./evidence.md) - Where it has held: origin in the series, the empty-value incident, the second truth.
* [Sources](./sources.md) - What this proposal builds on: the concept it imports, related proposals, external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent adopting this concept, including the mandatory step of deciding the choices with the user.
* [Extensions](./extensions.md) - Proposals that extend this one. None yet.

# History

* [Update log](./log.md) - Dated changes to this bundle.
