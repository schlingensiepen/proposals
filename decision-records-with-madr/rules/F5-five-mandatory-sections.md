---
type: Proposal Rule
description: Every record has Context and Problem Statement, Considered Options, Decision Outcome, Consequences, and Rejected and why, in this order; an empty section says why.
number: F5
binding: must
role: independent
applies_to: [writer]
---

# F5 · Five mandatory sections in fixed order

**Applies to:** writer.

**What holds.** Every record has, in this order: **Context and Problem
Statement** · **Considered Options** · **Decision Outcome** (with rationale and
trade-off) · **Consequences** · **Rejected and why**. The first four carry the
names of MADR 4.0.0; the deviations from MADR are named in the
[concept](../concept.md). No section is dropped; if one is empty, it says why.

**Why.** A fixed order makes the body readable crosswise — such records are
rarely read front to back; one jumps to the single piece of information needed
("what was the alternative again?"). That only works if the same information is
in the same place in every record. And a mandatory section takes effect exactly where
nobody thinks of it by themselves: **consequences** are almost always forgotten,
because they are uninteresting at the moment of deciding and the only thing that
matters a year later.

**Considered and rejected.** *Drop sections when there is nothing to say* — a
dropped section looks like a forgotten one; a justified empty one is itself a
statement. *Free structure per decision* — convenient in writing, expensive in
every reading, and after thirty records the body has thirty shapes.

**How you notice it is violated.** One has to read a whole record to find the rejected
alternatives.
