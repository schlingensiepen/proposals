---
type: Proposal
description: A decision record is based on MADR 4.0.0; two list fields in the head record what this document supersedes and what superseded it, and the concept names the deviations from MADR.
version: "1.1"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# Using MADR, without rebuilding it

This directory is an OKF bundle for one proposal of the series: the concrete form of a decision record. Anyone who sets out to document decisions usually invents a format first and notices after twenty records that it gets stuck in the same places as the finished one. MADR is that finished format; it costs nothing, everyone knows it, and a few named deviations make it cover revisions and rejected options cleanly too. The price is a somewhat verbose document and a head that is maintained by hand.

**Question.** What does a decision record actually look like: which fields, which sections, which file name?

**Thesis.** The format is based on MADR 4.0.0 instead of a homegrown one: the replacement relation is kept as two lists in the head, and the [concept](./concept.md) names the deviations from MADR.

The copyable template ships in [`examples/`](./examples/adr-template.en.md), in English and German.

## Start here

The [concept](./concept.md) describes the homegrown format and the lost replacement, and the form based on MADR 4.0.0 with its deviations; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Ten rules: format F1 to F3, file and structure F4 to F7, upkeep F8 to F10.

* [F1 Based on MADR 4.0.0](./rules/F1-madr-adopted-unchanged.md)
* [F2 The only addition to the head: both replacement directions as lists](./rules/F2-both-replacement-directions-as-lists.md)
* [F3 The head is bookkeeping, never content](./rules/F3-the-head-is-bookkeeping.md)
* [F4 The file name carries number and descriptive title](./rules/F4-file-name-carries-number-and-title.md)
* [F5 Five mandatory sections in fixed order](./rules/F5-five-mandatory-sections.md)
* [F6 The options appear twice](./rules/F6-options-appear-twice.md)
* [F7 Head keys in English, prose in one language](./rules/F7-english-head-keys-one-prose-language.md)
* [F8 A template lives in the body and is copied](./rules/F8-a-template-is-copied.md)
* [F9 The supersession notice is at the opening](./rules/F9-supersession-notice-at-the-opening.md)
* [F10 The index is a table showing standing](./rules/F10-the-index-shows-standing.md)

## Choices

This concept has no choices yet; every entry is a rule.

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled; the [evidence](./evidence.md) shows a body of thirty-seven records, the template in full and one replacement followed through. The [sources](./sources.md) name the mechanism this concept is the form of and the external sources on MADR; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for writing, replacing or reviewing decision records. The templates in [English](./examples/adr-template.en.md) and [German](./examples/adr-template.de.md) are copied as is; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle. This is version 1.1 of 2026-11-01: the thesis and F1 now say "based on MADR 4.0.0" instead of "unchanged", the concept names the deviations, the section names of F5 follow MADR 4.0.0, and the template quotes the numbers in the relation lists.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
