---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, version, what it imports.
* [Concept](./concept.md) - The chain that breaks silently, the concept of placing every check before publishing or in what the operator holds, its neighbors, its load-bearing structure, and the procedure for builder and operator.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. The model F1, duties before publishing F2 to F5, handover to the operator F6 to F7, outer edge F8.

* [F1 In a pull, nobody stands at the target](./rules/F1-nobody-stands-at-the-target.md) - Two roles and six actions; every assurance a push script would give at the target sits before publishing or in the artifact and its deployment file.
* [F2 The smoke-test checklist](./rules/F2-the-smoke-test-checklist.md) - Before publishing: entry point, own health check, every interface, self-description, writable state, first-start secret once, restart, recreation, reported build; throwaway values, a red run for each assertion.
* [F3 Names are computed, not formed](./rules/F3-names-are-computed-not-formed.md) - A computation that knows the registry's rules produces the names; every build gets an immutable name carrying the source revision beside the moving ones.
* [F4 latest is a promise](./rules/F4-latest-is-a-promise.md) - The name for whoever names none is written by the default branch's build alone, or not at all.
* [F5 Visibility is measured](./rules/F5-visibility-is-measured.md) - An anonymous fetch of the manifest under the name the deployment file uses, after every visibility change, not an inference from a setting.
* [F6 The header of the deployment file](./rules/F6-the-header-of-the-deployment-file.md) - Which image and what kind of name, login, pull / start / check steps, what travels in clear text, pass-through variables, where state lives.
* [F7 Rolled out is not running](./rules/F7-rolled-out-is-not-running.md) - Success is a call against the running instance that reports its build, compared with the intended one; pull explicitly before the start.
* [F8 What the outer edge advertises must get through](./rules/F8-what-the-outer-edge-advertises-must-get-through.md) - Measure every advertisement from outside with a client that follows it; switch off at the edge what the network blocks.

# Choices

No choices in this version; every entry is a rule.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when the answer flips, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - Where it has held: the build pipeline and deployment files of the author's mail-filing service, built, incidents, not met, origin.
* [Sources](./sources.md) - What this proposal builds on: the concepts it imports, the alternative and a related proposal, external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.
* [Example: an operator pulls, starts and asks](./examples/an-operator-pulls-starts-and-asks.md) - A walk-through with invented values, including a fix that the cache hides.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent acting as builder or operator: procedure, rules, acceptance checklist, common mistakes, self-check.
* [Extensions](./extensions.md) - Proposals that extend this one. None yet.

# History

* [Update log](./log.md) - Dated changes to this bundle.
