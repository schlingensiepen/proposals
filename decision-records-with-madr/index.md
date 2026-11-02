---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, where the template is.
* [Concept](./concept.md) - The problem of the homegrown format and the lost replacement, the concept based on MADR 4.0.0 with its deviations, its load-bearing structure, where the procedures are.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. Format F1 to F3, file and structure F4 to F7, upkeep F8 to F10.

* [F1 Based on MADR 4.0.0](./rules/F1-madr-adopted-unchanged.md) - The format is based on MADR 4.0.0, Markdown with a YAML head and its sections in its order; not renamed, not reordered, not adapted beyond the deviations named in the concept.
* [F2 The only addition to the head: both replacement directions as lists](./rules/F2-both-replacement-directions-as-lists.md) - The head carries supersedes and superseded-by, always as lists, so that a record found on its own can say it was superseded and merges and splits stay expressible.
* [F3 The head is bookkeeping, never content](./rules/F3-the-head-is-bookkeeping.md) - Status, decision date, participants and the two relation lists only; no rationale, summary or decision.
* [F4 The file name carries number and descriptive title](./rules/F4-file-name-carries-number-and-title.md) - `<nnnn>-<short-title>.md`, lowercase ASCII; never renamed after acceptance, a number never reused.
* [F5 Five mandatory sections in fixed order](./rules/F5-five-mandatory-sections.md) - Context and Problem Statement, Considered Options, Decision Outcome, Consequences, Rejected and why; an empty section says why.
* [F6 The options appear twice](./rules/F6-options-appear-twice.md) - A brief list with the chosen one marked, and a spelled-out reason for each rejected one.
* [F7 Head keys in English, prose in one language](./rules/F7-english-head-keys-one-prose-language.md) - Head keys stay as MADR prescribes; the prose is in one language across the whole body.
* [F8 A template lives in the body and is copied](./rules/F8-a-template-is-copied.md) - New records are copied from the template, not rebuilt from memory or from the last record.
* [F9 The supersession notice is at the opening](./rules/F9-supersession-notice-at-the-opening.md) - Below the heading of the superseded record, before its first section; nothing else in its text changes.
* [F10 The index is a table showing standing](./rules/F10-the-index-shows-standing.md) - One row per record, superseded rows struck through with a pointer, updated in the same move.

# Choices

This concept has no choices yet; every entry is a rule.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when this no longer holds, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - Where it has held: a private body of thirty-seven records, the template in full, one replacement followed through.
* [Sources](./sources.md) - What this proposal builds on: no imports, the mechanism it is the form of, external sources on MADR.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent writing, replacing or reviewing decision records: rules, three procedures, acceptance checklist, common mistakes, self-check.
* [Template, English](./examples/adr-template.en.md) - Template, copy as is: the MADR record with every head field and section and embedded format notes; keeps its own MADR head, no bundle header.
* [Template, German](./examples/adr-template.de.md) - Template, copy as is: the same template in German, for a body whose prose is German; keeps its own MADR head, no bundle header.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
