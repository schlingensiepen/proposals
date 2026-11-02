---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, version, the parts of the bundle with links.
* [Concept](./concept.md) - The problem, the concept in prose with its three components and its load-bearing structure, the order in which the rules are applied.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. F1 is the basic rule and the check question; F2 to F4 concern the structure; F5 is the inlining rule for decisions; F6 and F7 concern maintenance.

* [F1 The check rule: does it work with dead links?](./rules/F1-the-dead-link-test.md) - Every document stays understandable and followable when every link to the outside is dead; outside knowledge is brought in with statement and reasons; redundancy is allowed.
* [F2 An entry point that branches instead of explaining](./rules/F2-an-entry-point-that-branches.md) - Exactly one file is read first; it names the two or three actual tasks and points to the instruction for each; further entry files of other tools only point to it.
* [F3 One task definition per recurring activity](./rules/F3-one-task-definition-per-recurring-activity.md) - Every recurring activity has its own instruction: what to do, in which order, how to tell it is done.
* [F4 Instructions are self-contained, even toward their own repository](./rules/F4-instructions-are-self-contained.md) - Every rule and check question is written out in the instruction; numbers or sections of other documents appear only as provenance in parentheses.
* [F5 Decisions come into the document with their reasons](./rules/F5-decisions-come-with-their-reasons.md) - Statement and reasons, and where it carries the rejected alternatives, are taken into the document; a link to the decision log is a maintenance note only; in a generated document they are generated anew from the source.
* [F6 Redundancy is maintained, not avoided](./rules/F6-redundancy-is-maintained.md) - Every change asks which other places it touches and handles them in the same change; the main pairings are written down.
* [F7 What is generated is marked as generated, with its source named](./rules/F7-generated-is-marked-as-generated.md) - Generated files carry a visible note and name their leading source; changes happen only at the source; generation runs automatically.

# Choices

This version has no choices; every entry is a rule.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when this no longer holds, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - Where it has held: the proposal series of the author as a running application, an unplanned test by a context-free agent, the origin of the inlining rule and of F4.
* [Sources](./sources.md) - What this proposal builds on: the imported proposal, related proposals and external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent that makes a repository self-contained or reviews one: procedure with the dead-link test by a reader from outside, duties of adoption, checklist, common mistakes.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
