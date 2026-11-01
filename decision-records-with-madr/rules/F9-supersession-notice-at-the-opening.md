---
type: Proposal Rule
description: A superseded record gets a visible notice pointing at its successor below its heading, before the first section; nothing else in its text changes.
number: F9
binding: must
role: load-bearing
applies_to: [writer]
---

# F9 · The supersession notice sits at the opening of the superseded record

**Applies to:** writer.

**What holds.** When a record is superseded, a visible notice with a pointer to
the successor goes below its heading, **before** the first section. It is, next
to the head, the second and last place where an accepted record is still
changed. Nothing else in the text changes.

**Why.** The backward pointer in the head (F2) is machine-readable and
practically invisible to people: whoever opens a record reads the first lines of
the **text** — not the metadata block, and certainly not a field near its
bottom. A supersession recorded only in the head is formally correct and still
gets missed; and it gets missed on precisely the record a reader finds first,
because that one is older, more detailed and linked more often.

That both interventions — head and opening — are permitted at all has a
compelling reason: **the backward pointer comes into being later than the record
carrying it**, often years later. Without this exception the replacement could
only be entered forwards, in the new record, and the old one would know nothing
of its own end. Neither intervention touches the statement: both record *what
happened to* the document, not *what it says*.

**Obvious and rejected.** *Maintain the head only* — machine-correct,
practically invisible; see above. *Put the notice at the end* — there it sits
behind the very thing it warns about. *Shorten the superseded record's text or
replace it with a pointer* — destroys exactly the evidence of what the decision
was and why it seemed right at the time; the superseded record is the only place
that holds it.

**How you notice it is violated.** Someone cites a decision from a record that has been
superseded for months — and has never seen its head.
