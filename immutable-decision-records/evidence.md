---
type: Proposal Evidence
description: Where the mechanism has held; its origin in the architecture decision record practice, the author's own body of records migrated out of a collection file, and one decision followed through its replacement.
sources:
  - id: nygard-adr
    url: https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions
    title: Documenting Architecture Decisions (2011)
---

# Evidence

## Origin

**This mechanism is not new, and that is its best evidence.** It is the core of what has been practiced since 2011 as an *Architecture Decision Record*: the original description already demands that an accepted record not be changed but replaced by a new one (see the sources). Anyone who has worked with ADRs knows the procedure, usually as a quirk of a format, though, not as a self-standing answer to a question.

That is precisely why it stands here as a proposal of its own. The question "how do I keep decisions traceable?" is **not confined to architecture decisions**; it arises just as much for product, procurement, operational and editorial decisions, where the mechanism helps while the vocabulary ("architecture decision record") misleads and the accompanying file format is oversized. What this proposal contributes is not the invention but the **separation of the mechanism from the format**: it applies to any decision log, and rules F1 to F8 say which parts of it are load-bearing and which are not.

## A body of records migrated out of a collection file

The author's own experience with it comes from a private project with a body of thirty-seven records, nine of them superseded. It did not start on a green field but arose from a **migration out of a collection file**, and that migration is the most solid experience with it, because it exposed the weaknesses of the prior state: in the collection file, revisions had landed as additional paragraphs beneath the original decision ("revision of …"), so that several entries were valid and invalid at the same time, depending on the paragraph.

Two experiences from that migration sharpened rules in this concept:

- The executing editor found a **wrong total** in the migration plan: the plan demanded a record in its prose that its own sum failed to count. Whoever had followed the number instead of the text would have silently lost a decision and its rationale. Hence the rule for every migration: **when in doubt, keep.**
- A superseded record had initially been given its successor's title so the index would look "consistent". The result was two identically named entries and ambiguous citations; hence F4.

## One decision, followed through

> **Example** (from the same project, followed through): for the items of an inventory system it was decided that their **visible number is an identity attribute**: it identifies the item, is unique, and does not change. The record was titled "Visible number: identity attribute, not history data", rationale: whoever reads a number should find exactly one item with it.
>
> Half a year later it turned out that in practice numbers are assigned afterwards and corrected: labels fall off, numbering schemes get reorganized. The decision was wrong. What followed was **not an added paragraph** in the old record but a new one with its own title ("Visible number: an attribute of its own, versioned in the history"), its own complete rationale, and its own list of rejected alternatives, including the old decision itself, now with the reason it failed.
>
> The head of the new record reads `supersedes: [0011]`, the head of the old one `superseded-by: [0031]` and `status: superseded`; in the index the line for 0011 is struck through and carries the note "↳ superseded by 0031". Whoever searches for the numbering scheme today finds both records, and knows within two seconds which one applies and that there was a predecessor given up for a good reason.

## State of play

The status of this proposal stays **draft**: a body of this size and a few months of operation are too little for "proven". In particular, the condition under which the answer flips because decisions change weekly has not been experienced yet.
