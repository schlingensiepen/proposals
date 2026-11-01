---
type: Proposal Evidence
description: Where the format has held; the author's private body of thirty-seven records migrated out of a collection file, the template in full, and one replacement followed through.
sources: []
---

# Evidence

## Origin

The format runs in a private project of the author with thirty-seven records, nine of them superseded. It did not start on a green field but arose from a **migration out of a collection file** in which revisions had landed as extra paragraphs beneath the original decision. That migration forced two rules in this dossier: F2, because three records surfaced that each merged two predecessors, and with a single-valued field one of them would have had to be concealed; and F8, because the first hand-rebuilt records each turned out to be missing a different head field.

## The template

The template ships with this bundle, English as [`examples/adr-template.en.md`](./examples/adr-template.en.md), German as [`examples/adr-template.de.md`](./examples/adr-template.de.md). It is the actual subject of F8: copy, rename, fill in. Its wording, reproduced in full here so this dossier stays usable without the attachment:

```markdown
---
status: draft              # draft | accepted | superseded
date: YYYY-MM-DD           # the day it was decided, not the day it was written
decision-makers: []        # who decided — roles rather than personal names
consulted: []              # who was asked
informed: []               # who needs to know
supersedes: []             # numbers this record replaces — ALWAYS a list
superseded-by: []          # numbers that replaced this record — ALWAYS a list
---

# ADR-<nnnn> · <Title: the decision, not the topic>

## Context and problem

<Where the question comes from. What triggered it, what went wrong, what was
pending? Not "this record discusses X", but the problem in the language of the
person who had it.>

## Considered options

- **<Option A>** (chosen)
- **<Option B>** — rejected
- **<Option C>** — rejected

## Decision

**Decision:** <One indicative sentence, quotable, without justification.>

**Rationale:** <The reason, not the derivation.>

**Trade-off:** <What spoke for the other side and why it was not enough. Whoever
cannot write anything here did not take the alternative seriously.>

## Consequences

<Positive and negative, both spelled out. The negative ones are why this section
is mandatory — they look unimportant at the moment of deciding and are the only
thing that matters a year later.>

## Rejected and why

**<Option B>:** <The spelled-out reason. This section is the value of the whole
body: it prevents the rejected solution from being proposed again.>

**<Option C>:** <…>
```

## A replacement, followed through

> **Example** (from the same project, followed through): for the items of an inventory system it was decided that their visible number is an identity attribute. The record was called `0011-sichtbare-nummer-identitaetsattribut.md`, its head carried `status: accepted`, `date: 2026-08-07`, both relation lists empty.
>
> Half a year later it turned out that numbers are assigned afterwards and corrected. `0031-sichtbare-nummer-versioniert.md` came into being with `supersedes: [0011]`; in `0011`, `status` was set to superseded, `superseded-by: [0031]` was added, and a notice pointing at the successor was placed above the heading. Nothing in the prose of `0011` was changed; it still describes the decision of 2026-08-07, including the rationale that seemed right at the time.
>
> In the index the line for `0011` is struck through with the note "↳ superseded by 0031", the line for `0031` with "supersedes 0011". Both directions are thus kept in three places: in both heads and in the index.

The file names are those of the original body, which is written in German.

## Standing of this proposal

The status of this dossier stays **draft**: a body of this size and a few months of operation are too little for "proven". In particular, the second condition under which the answer flips (see the scope), hand-kept head fields no longer matching, has not been experienced yet.
