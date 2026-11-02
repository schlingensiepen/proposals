---
type: Proposal Scope
description: Security considerations of a documentation method, what the mechanism costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

A documentation method appears security-neutral. It is not: immutability also covers what got in by accident, and a misread status can put an abandoned protective measure back into force.

| Threat | Typical course of events | Countermeasure |
|---|---|---|
| **A secret becomes immutable** | An example in the record contains a real key, hostname or credential. It is noticed later, but the text may no longer be changed | Check before acceptance; use invented values in examples as a matter of course. If it happens anyway, **secret rotation takes precedence over immutability**: the key is rotated, and removing it from the text is handled as an explicit, logged exception, not as a silent correction |
| **A superseded security decision is implemented** | Someone searches the set of records for "access" and hits the older, more detailed record first. It describes an open read access that was abandoned long ago | Status marker **inside the record itself**, not only in the index (F5), and visible at the top, not in the head block alone. A reader who only reads the opening must see the supersession |
| **An assistant system pulls in superseded decisions** | A tool reads the whole folder into its context and weighs all records equally | Index with unambiguous marking (F8) plus an entry note saying explicitly that superseded records are kept; otherwise their mere presence reads as validity |
| **Retroactive reinterpretation** | A participant later edits the rationale of a decision that has become inconvenient | Immutability is itself the countermeasure here, provided the set of records is version-controlled: every change is visible as such |

**What the concept does not protect against:** it does not stop a decision from being factually wrong, and it is no substitute for access control. A set of decision records routinely reveals more about a system's architecture than its source code; anyone who publishes it should do so on purpose.

## What it costs

**More documents.** A set of records that would have twenty sections when amended has forty files here, a third of which no longer apply.

**One sentence of change costs a whole record.** This is the weak spot: the effort is noticeable, and the temptation to "just quickly" correct the old record is greatest at exactly this spot, with small changes, where the method feels most expensive.

**Redundancy between predecessor and successor.** Large parts get copied over. If you read both, you read much of it twice.

**The index is a duty one can forget.** If it is forgotten, the benefit of the whole method collapses without anything looking broken.

**The chain has to be read.** Understanding the prehistory of a decision means opening three records instead of one paragraph.

## Where it applies

### Preconditions

The set of records is kept as text files in a version-controlled directory, one file per decision, with a machine-readable head. Nothing more is required: no tool, no database, no web front end. Anyone who writes must be willing to write a record to the end rather than sketch it.

### When this no longer holds

**Decisions change weekly.** Then you replace more than you decide, and the set of records turns into a chain without substance.

**The decisions are not decisions at all** but external mandates one merely notes. Then the core is missing (F7), and a plain list will do.

**The set of records grows so large that a hand-kept index no longer matches.** Beyond that point F8 needs tool support, or the overview itself becomes the error source.

### What it does not solve

This concept says nothing about **what** to build; it replaces neither a specification nor requirements documentation. Nor does it describe how a set of records **comes into being** when decisions were made verbally for years; reconstructing them afterwards is a separate task with its own traps. And it does not govern who is allowed to decide.

### Not yet settled

* Beyond what size a hand-kept index stops working. The author's guess is around fifty records; this is not substantiated. Experience from larger sets of records is sought.
* How to handle a decision that is very long and of which only a small part changes. F3 demands a complete rewrite, and for a ten-page record that is a real hurdle, with the risk that the decision therefore goes *undocumented*. Whether the right answer is "such records are too big anyway and should have been split", or whether an exception is needed here, is open.
* Whether there may be a record that collects **several** early decisions never documented individually, as a chronicle at the start of a set of records. This plainly violates F1. The author allowed it because the alternative would have been to invent a dozen decisions retroactively for which no reliable memory existed. Whether that is a legitimate exception or the first crack is open.
* How to keep pointers from source code to decision numbers current when a decision is superseded. A comment pointing at the old number is not wrong under F6, but it does point at the superseded record.
