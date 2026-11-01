---
type: Proposal Rule
description: The head holds status, decision date, participants and the two relation lists only; no rationale, summary or decision, because it is the part that may still change after acceptance.
number: F3
binding: must
role: load-bearing
applies_to: [writer]
---

# F3 · The head is bookkeeping, never content

**Applies to:** writer.

**What holds.** The head holds administrative data only: status, decision date,
participants, the two relation lists. No rationale, no summary, no decision.

**Why.** The content of a record is immutable after acceptance — statement,
rationale, options and consequences are not touched again, because otherwise it
is no longer datable what applied when. The head is one of exactly two
exceptions to that: the status flips, the backward pointer is added (the second
is the supersession notice, F9). If the head also held content, that exception
could no longer be drawn cleanly, and immutability would be void in practice:
whoever "updates" a summary in the head has changed
the statement without touching the text. Conversely, what sits in the head is
evaluated by tools and must therefore carry no nuance that only a human
understands.

**Obvious and rejected.** *A one-line summary in the head so overviews can show
it* — tempting, but it is content and ages with it; an index can show the title,
and the title is in the heading. *Tags in the head* — harmless while they only
sort; as soon as someone infers the subject from them, they replace the text.

**How you notice it is violated.** The decision can be answered from the head alone — or a
head field was changed because the situation changed, not the status.
