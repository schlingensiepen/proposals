---
type: Proposal Concept
description: The problem of inventing a decision-record format and losing track of replacements, the concept based on MADR 4.0.0 and its deviations from it, its load-bearing structure, and where the procedures live.
---

# The concept

## Problem

"I understand that I should write my decisions down. But what does the thing look like? What goes at the top, what belongs in the text, what is the file called, and how do I later recognize that this decision has been superseded by another one? I start inventing a format, and by the third record I no longer know whether I did it the same way in the first."

Two different questions hide behind this and are readily mixed up. One is about **content**: what must a decision contain so that it still holds in two years? The other is about **form**: what does the file look like so that a body of a hundred records stays searchable, sortable and machine-readable. This dossier answers the formal question, and answers it in a way that does not work against the other one.

The sore point is precisely the place no widespread format solves satisfactorily: **replacement**. Decisions get revised; if the format provides only a single field for that, or none, the information ends up in the prose, where no tool finds it and no reader sees it reliably.

## The concept

Every decision is a Markdown file **based on MADR 4.0.0**: a YAML head with status, date and participants, below it context and problem statement, considered options, the decision outcome with its rationale, and the consequences. The file name starts with a four-digit number followed by a descriptive short title in lowercase. Above all records there is an index as a table. And a template lives in the body, to be copied rather than rebuilt from memory.

**Where this form deviates from MADR 4.0.0:**

* **Head lists.** The head gains two fields, `supersedes` and `superseded-by`, both as **lists**, because a revision can merge several decisions and one decision can split into several (F2).
* **Additional section.** One mandatory section, "Rejected and why", spells out the rejected alternatives with their reasons, so the options appear twice, once as a list for skimming, once as a paragraph with justification (F5, F6).
* **Consequences.** In MADR this is an optional subsection of Decision Outcome; here it is a mandatory section of its own (F5).
* **Status values.** MADR uses proposed, rejected, accepted, deprecated and superseded by a named record; here the values are draft, accepted and superseded.
* **Date.** In MADR the date is the last update; here it is the day of the decision (F3).

**These records are subjected to a mechanism that is older than the format and does not belong to it:** an accepted record is not changed in content, a revision is a new record replacing the old one, and the old one stays. That is the actual point of the exercise, the traceability of a sequence of decisions, and it is at the same time the reason decision records exist at all. This dossier describes the **form** that mechanism takes in MADR; wherever a rule here rests on it, it is spelled out (the term "accepted", F3, F9). Its full justification as a mechanism in its own right, including for decision logs without MADR and without any architecture context, is a subject of its own; the sources name it. **It is not a prerequisite:** what this dossier needs is in this dossier.

**Load-bearing structure.** F1 carries F3 and F5: both are MADR's own shape, not our invention; anyone who gives up F1 must justify them anew. F2, F9 and F10 belong together: they carry the same replacement in three places, one per access path; if one falls, the replacement stays true but becomes invisible to part of the readership. F3 rests on the immutability of the content; without it, what is in the head would not matter. F4, F7 and F8 are craft and independent of one another, each adoptable separately. F6 is a readability decision; dropping it loses convenience, not substance. **If you take only one rule, take F8**: the template silently carries all the others.

## The procedure

The rules say what must hold. How one proceeds is written out step by step in the agent guide, [`AGENT.md`](./AGENT.md), in three procedures: writing a new record, recording a replacement, and reviewing an existing body. The template the first procedure starts from is in [`examples/`](./examples/adr-template.en.md).
