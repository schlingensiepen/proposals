---
status: draft              # draft | accepted | superseded
date: YYYY-MM-DD           # the day it was decided, not the day it was written
decision-makers: []        # who decided — roles rather than personal names
consulted: []              # who was asked
informed: []               # who needs to know
supersedes: []             # numbers this record replaces — ALWAYS a list
superseded-by: []          # numbers that replaced this record — ALWAYS a list
---

<!-- TEMPLATE: copy, rename to `<nnnn>-<short-title>.md`, fill in. Do not take the
     most recently written record as a model — that is how deviations propagate.

     HEAD RULE: the head holds bookkeeping only. No summary, no rationale, no
     decision. The head is the only place that may still be changed after
     acceptance (status, relation lists) — if it held content, the immutability
     of the content would be void in practice.

     BOTH RELATION FIELDS ARE LISTS, even with a single entry: a revision can
     merge two decisions, and one decision can split into two.

     FILE NAME: <nnnn>-<short-title>.md — four-digit number, lowercase, hyphens,
     ASCII only. After acceptance the name is not changed; numbers are never
     reused and gaps are left open.

     EXAMPLES USE INVENTED VALUES ONLY. A real key or hostname inside an accepted
     record can no longer be removed quietly. -->

# ADR-<nnnn> · <Title: the decision, not the topic>

<!-- The title states what was DECIDED ("Storage stays SQLite"), not the topic
     area ("Storage"). It stays as it is even when the record is superseded later
     — otherwise two identically named entries sit in the index and citations
     become ambiguous.

     IF THIS RECORD IS SUPERSEDED, a visible notice goes here — above "Context and
     problem" — pointing at the successor. Together with the head, this is the
     second and last exception to immutability: the head is read by tools, this
     notice by people. -->

## Context and problem

<Where the question comes from. What triggered it, what went wrong, what was
pending? Not "this record discusses X", but the problem in the language of the
person who had it.>

## Considered options

<A brief list for skimming, with the chosen one marked. The spelled-out reasons
live below in "Rejected and why" — the two sections serve different readers and
do not replace one another.>

- **<Option A>** (chosen)
- **<Option B>** — rejected
- **<Option C>** — rejected

## Decision

**Decision:** <One indicative sentence, quotable, without justification.>

**Rationale:** <The reason, not the derivation. Without it the decision is
abandoned at the first pushback.>

**Trade-off:** <What spoke for the other side and why it was not enough. Whoever
cannot write anything here did not take the alternative seriously.>

## Consequences

<Positive and negative, both spelled out. The negative ones are why this section
is mandatory: they look unimportant at the moment of deciding and are the only
thing that matters a year later.>

## Rejected and why

<One paragraph per rejected option with the reason spelled out. This section is
the value of the whole body of records — it prevents the rejected solution from
being proposed again, and it is the only place the reason survives once everyone
has forgotten it. If an option appears in the list above but is missing here, it
is unclear whether it was examined or forgotten.

If nothing else was ever on the table: "no alternative considered, because …" —
never simply omit it.>

**<Option B>:** <…>

**<Option C>:** <…>
