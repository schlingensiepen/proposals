---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, the parts of this bundle.
* [Concept](./concept.md) - The problem of two implementations of the same operation that drift apart unnoticed, the concept with its load-bearing structure, the three-step procedure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. F1 and F2 belong together: F1 says that every operation exists once, F2 how that is held.

* [F1 One operation, many pass-throughs](./rules/F1-one-operation-two-pass-throughs.md) - Every action exists once as a function without HTTP or exit knowledge; each surface has one translation function and its presentation, and both emit the same field names; no surface touches store, file system or target system.
* [F2 Oneness is checked structurally](./rules/F2-oneness-is-checked-structurally.md) - A structural test over the pass-throughs: whitelist of imports, banned operation names, no file system access except named exceptions, every command and handler calls exactly one operation; each assertion seen red.

# Choices

This concept has no choices.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, the precondition, what is not yet settled; parts not yet written.
* [Evidence](./evidence.md) - Where it has held: the origin, the reference implementation with what it evidences and what is only partly met, the incident of the ten drifted places.
* [Sources](./sources.md) - What this concept builds on: no imports, one recommended companion, the reference implementation.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for builders: procedure, rules, acceptance checklist, common mistakes, self-check.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
