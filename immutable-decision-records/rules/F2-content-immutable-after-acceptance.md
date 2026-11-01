---
type: Proposal Rule
description: After acceptance the content of a record is immutable; only the head and the supersession notice at the opening stay editable, plus typos and dead links.
number: F2
binding: must
role: load-bearing
applies_to: [author]
---

# F2 · After acceptance the content is immutable, head and document opening excepted

**Applies to:** author.

**What holds.** What is immutable is the **content**: the statement of the decision, its rationale, its options, its consequences. **Two places are expressly excepted**: the **head** (status and the two relation lists) and the **opening of the document**, where the supersession notice with the pointer to the successor sits. Typographical errors and dead links may be fixed as well. Everything else is a new record.

**Why.** Immutability is what makes a statement datable. If a record from 2024 contains a sentence from 2026, not a single statement in it can be placed in time, and therefore no rationale either, because a rationale always holds relative to a state of knowledge.

**The two exceptions are not convenience but the precondition for keeping the relation in both directions.** The backward pointer from the superseded record to the superseding one necessarily comes into being **later** than the superseded record itself, often years later. Without this exception the replacement could only be entered forwards, and a superseded record would not be recognizable as superseded on its own (F5). Yet that is precisely the record a reader hits first.

That there are **two** places and not one has a reason: the head is read by tools, the notice at the opening by people. Whoever opens a record reads the first lines of the text, not the metadata block, and certainly not a field at the very bottom. A supersession recorded only in the head is machine-correct and practically invisible.

Neither intervention touches the state of knowledge: both record *what happened to* the document, not *what it says*. The notice at the opening says nothing about the decision; it says that it no longer applies.

**Obvious and rejected.** *Allow small clarifications, forbid larger ones*: the boundary cannot be determined, and everyone draws it so that their own change still fits inside. Half a year later the principle has softened without anyone having decided so. *Freeze head and notice as well, so the rule needs no exception*: sounds cleaner and makes the supersession one-sided: it would live only in the new record, and the old one would know nothing of its own end. *Allow changes and leave the history to version control*: it is stored there, but nobody reads it: whoever opens a record sees the latest state, and the question "has it always said this?" never arises.

**How you notice it is violated.** A record carries an acceptance date, and its last change is later than that and concerns something other than the head, the supersession notice, a typo, or a dead link.
