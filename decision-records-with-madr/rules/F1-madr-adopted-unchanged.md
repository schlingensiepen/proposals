---
type: Proposal Rule
description: The format is based on MADR 4.0.0, Markdown with a YAML head and its sections in its order; not renamed, not reordered, not adapted beyond the deviations named in the concept.
number: F1
binding: must
role: load-bearing
applies_to: [writer]
---

# F1 · Based on MADR 4.0.0

**Applies to:** writer.

**What holds.** The format is based on MADR version 4.0.0 — Markdown with a YAML
head, sections in the order it prescribes. It is not renamed, not reordered and
not "adapted to our needs" beyond the deviations named in the
[concept](../concept.md).

**Why.** A homegrown format costs three times: inventing it, explaining it, and
migrating away from it when a tool eventually appears that expects a common
format. MADR is common enough that tools and people know it, and lean enough not
to get in the way. Above all it is **finished**: the questions where a homegrown
format gets stuck after twenty records — where do the options go, where the
consequences, what belongs in the head — are already decided there.

**Considered and rejected.** *A minimal format of one's own ("title, reason,
done")* — faster at the start, but it lacks exactly the fields that get expensive
later: status, date, options. *Nygard's original format without a YAML head* —
historically the root and still viable, but without a machine-readable head no
index can be generated and no status queried. *An ADR tool with its own format
and storage* — binds the body to a program; the records should stay readable and
writable with a text editor.

**How you notice it is violated.** Two records in the same body carry different section
headings, and nobody knows which one was the template.
