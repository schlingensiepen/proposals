---
type: Proposal
description: An accepted decision record is never changed again; every change is a new, complete record that replaces the old one, and both ends of the replacement are kept.
version: "1.1"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# Decisions are replaced, not edited

This directory is an OKF bundle for one proposal of the series: a mechanism for any decision log, not only for architecture decisions. In one sentence: anyone who wants to change a decision already made writes a new, self-contained record that replaces the old one completely, even if only one sentence changes.

**Question.** How do you record decisions so that they still hold in two years, and so that you can see what no longer applies?

**Thesis.** An accepted decision record is never changed again; every change is a new, complete record that replaces the old one.

## Start here

The [concept](./concept.md) describes the problem of amended decision records and the immutable record replaced as a whole; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Eight rules: F2 carries everything, F1 is its precondition, F3 to F6 govern what a replacement looks like, F7 and F8 are independent.

* [F1 One record, one decision](./rules/F1-one-record-one-decision.md)
* [F2 After acceptance the content is immutable](./rules/F2-content-immutable-after-acceptance.md)
* [F3 Replacement is complete](./rules/F3-replacement-is-complete.md)
* [F4 The superseded record stays](./rules/F4-superseded-record-stays.md)
* [F5 The replacement relation is n:m and kept at both ends](./rules/F5-relation-n-to-m-at-both-ends.md)
* [F6 Numbers are never reused](./rules/F6-numbers-are-never-reused.md)
* [F7 The rejected alternatives are mandatory](./rules/F7-rejected-alternatives-are-mandatory.md)
* [F8 An index shows the status](./rules/F8-an-index-shows-the-standing.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled; the [evidence](./evidence.md) shows the origin in architecture decision records, a set of records migrated out of a collection file and one decision followed through its replacement. The [sources](./sources.md) name the proposal that gives this mechanism a form and the external sources; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for writing, revising, migrating or reading decision records; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
