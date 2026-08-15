---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-13
author: Jörn Schlingensiepen
slug: immutable-decision-records
language: en — translated from the German original
sync: v1.0 (2026-08-13)
question: "How do you record decisions so that they still hold in two years — and so that you can see what no longer applies?"
thesis: "An accepted decision record is never changed again; every change is a new, complete record that replaces the old one."
---

# Decisions are replaced, not edited

**In one sentence:** Whoever wants to change a decision already made writes a
new, self-contained record that replaces the old one completely — even if only
one sentence changes.

**Abstract:** Decision records get amended in day-to-day work, and that is
exactly how they lose their value: in the end you see only the latest state and
no longer know what applied when, or why. Immutability plus complete
replacement restores both — today's state *and* the path to it — at the price
of more documents and some redundancy.

## 1. Problem

"I decided a year ago to build it this way. That I decided it, I still know.
What I had against the obvious alternative back then, I no longer know — so I
think it through again, arrive with the same head at mostly the same result,
just three days later. And when I open the record, there is a sentence someone
corrected at some point. Whether that was the original decision or a later
repair, I cannot see."

That is the normal case with amended decision records. Two distinct kinds of
damage hide in it:

- **The lost reason.** A decision without its rejected alternatives is an
  opinion. It is abandoned at the first pushback — by a new colleague, by an
  assistant system, by one's own self two years on.
- **The invisible switch.** When a record is corrected in one place, the result
  is a text that *looks* like one decision but in truth layers two decisions
  from different years on top of each other. Nobody can see where the seam runs.

The second damage is the more expensive one, because it does not announce
itself. A missing reason is annoying; an invisible seam leads someone to
implement a long-revised decision while believing they follow the current state.

## 2. The concept

Every decision gets its own short record with a sequential number. While it is a
draft, it is worked on. The moment it is accepted, its content becomes
**immutable**: nobody edits the prose again. Whoever wants to revise the
decision writes a **new** record that answers the question completely afresh —
with its own rationale, its own rejected alternatives, its own title — and
records in the head of both documents which replaces which. The old one stays,
is marked as superseded, and is never deleted. An index across the whole body
shows at a glance what applies and what is merely history. Numbers are never
reused, not even after a draft was discarded. This leaves exactly two questions
answerable at any time: *what applies today?* and *what applied back then, and
why was it given up?*

## 3. Terms

- **Decision record** — a text holding **one** decision that was made: the
  question, the options, the choice, the rationale, the consequences.
- **Accepted** — the state from which the decision applies and the text becomes
  immutable. Its counterpart is **draft**: there, writing, discarding and
  reordering still happen.
- **Replacement** — the complete supersession of one record by a new one. Not:
  a correction, an addendum, a footnote.
- **Superseded** — the state of a record whose decision no longer applies. It
  stays readable and findable.
- **Chain** — the sequence of records on the same subject, linked by
  replacement markers.
- **Index** — the single overview of the entire body, from which the standing of
  every record follows.
- **Head** — the machine-readable block at the top of the file (status, date,
  participants, replacement relations). It is bookkeeping, not statement.

## 4. Decisions

### F1 · One record, one decision

**What holds.** Every decision lives in its own file with its own sequential
number; there is no collection file holding all decisions.

**Why.** Only a separate file can apply as a whole or be superseded as a whole.
In a collection file, standing is an attribute of paragraphs — and paragraphs
carry no status that a tool or a reader picks up reliably. It is also the only
way to answer when a decision was made: the file carries its own date.

**Obvious and rejected.** *One collection file with a section per decision* —
convenient to search and tidy at first, but it forces amendment: a revision
becomes a correction inside a paragraph, which is precisely what this concept
sets out to prevent. *One record per topic area* ("everything about storage") —
the boundaries shift over time, and every revision then touches neighbouring
decisions nobody meant to reopen.

**Symptom of violation.** Someone asks "does the second paragraph still apply?"
— and nobody can answer it from the record.

### F2 · After acceptance the content is immutable — head and document opening excepted

**What holds.** What is immutable is the **content**: the statement of the
decision, its rationale, its options, its consequences. **Two places are
expressly excepted** — the **head** (status and the two relation lists) and the
**opening of the document**, where the supersession notice with the pointer to
the successor sits. Typographical errors and dead links are permitted as well.
Everything else is a new record.

**Why.** Immutability is what makes a statement datable. If a record from 2024
contains a sentence from 2026, not a single statement in it can be placed in
time — and therefore no rationale either, because a rationale always holds
relative to a state of knowledge.

**The two exceptions are not convenience but the precondition for keeping the
relation in both directions.** The backward pointer from the superseded record
to the superseding one necessarily comes into being **later** than the
superseded record itself — often years later. Without this exception the
replacement could only be entered forwards, and a superseded record would not be
recognizable as superseded on its own (F5). Yet that is precisely the record a
reader hits first.

That there are **two** places and not one has a reason: the head is read by
tools, the notice at the opening by people. Whoever opens a record reads the
first lines of the text — not the metadata block, and certainly not a field at
the very bottom. A supersession recorded only in the head is machine-correct and
practically invisible.

Neither intervention touches the state of knowledge: both record *what happened
to* the document, not *what it says*. The notice at the opening says nothing
about the decision — it says that it no longer applies.

**Obvious and rejected.** *Allow small clarifications, forbid larger ones* — the
boundary cannot be determined, and everyone draws it so that their own change
still fits inside. Half a year later the principle has softened without anyone
having decided so. *Freeze head and notice as well, so the rule needs no
exception* — sounds cleaner and makes the supersession one-sided: it would live
only in the new record, and the old one would know nothing of its own end.
*Allow changes and leave the history to version control* — it is stored there,
but nobody reads it: whoever opens a record sees the latest state, and the
question "has it always said this?" never arises.

**Symptom of violation.** A record carries an acceptance date, and its last
change is later than that and concerns something other than the head, the
supersession notice, a typo, or a dead link.

### F3 · Replacement is complete — there is no "partly superseded"

**What holds.** A replacing record answers the question completely afresh,
including the parts that remain unchanged. A marker of the kind "still applies,
except paragraph 3" is not permitted.

**Why.** Whoever reads "partly superseded" must hold two records in mind at once
and decide for themselves which sentence is hit by what. That is exactly the
work this concept means to take off the reader — and it is error-prone work,
performed anew by every reader, each time possibly with a different result. The
writer performs it once.

**Obvious and rejected.** *A delta record describing only the change* — saves
effort in writing and costs it in every reading; after the second revision you
need three records to answer one question. *A pointer saying "otherwise the
predecessor applies"* — the same trap in polite form: it makes the new record
incomplete, and when reading the chain nobody can tell which version of "the
rest" was meant.

**Symptom of violation.** A question can only be answered by laying two or more
records side by side.

### F4 · The superseded record stays — and keeps its own title

**What holds.** A superseded record is not deleted, not emptied and not renamed.
Its title keeps describing what **it** decided, not what its successor decides;
it receives a status marker and a pointer to the successor.

**Why.** The superseded record is the only place where the abandoned solution
and its rationale of the time are written down. Deleting it produces exactly the
state the concept avoids: a decision without a traceable prehistory. And its own
title matters more than it looks — if the old record carried the successor's
title, two records with the same name would sit in the index, and citing a
decision would be ambiguous.

**Obvious and rejected.** *Move superseded records into an archive directory* —
breaks every existing pointer to them, and the chain becomes unreadable as soon
as it crosses the directory boundary. *Pull the title up to the new state so the
index looks consistent* — creates duplicates in the register and makes citations
ambiguous; the register shows standing by other means anyway (F8).

**Symptom of violation.** Two entries in the index carry the same name — or a
pointer to an older decision leads nowhere.

### F5 · The replacement relation is n:m and is kept at both ends

**What holds.** Every record carries two **lists** in its head: what it
supersedes and what superseded it. Both are maintained in the same move that
creates the successor.

**Why.** The relation is not one-to-one. A revision can merge two old decisions
into one, and a growing decision can split into two; both occur once a body of
records is a few years old. A single-valued field would force a white lie in
those cases. And both directions are needed because both reading directions
occur: forwards ("what became of this decision?") and backwards ("what did this
record replace, and why was that not enough?").

**Obvious and rejected.** *Only the forward pointer in the new record* — then a
superseded record is not recognizable as superseded on its own, and that is
precisely the record a reader hits first when searching the body.
*Single value instead of list* — forbids merging and splitting; whoever needs
them later builds an ad-hoc note in the prose and has thereby put the relation
somewhere no tool will find it.

**Symptom of violation.** A record names its successor and the successor does
not name it — or the other way round.

### F6 · Numbers are never reused

**What holds.** A number once issued stays with its record, even if that record
was discarded while still a draft. Gaps in the sequence are normal and are not
closed.

**Why.** Numbers get cited — in meetings, in source comments, in other
documents, in mail. A reused number makes every earlier citation silently wrong:
the pointer still resolves to an existing record, just the wrong one. A dead
pointer is harmless; a redirected one is dangerous.

**Obvious and rejected.** *Close gaps so the sequence looks clean* — cosmetic,
and it usually shifts several numbers at once. *Drop numbers altogether and cite
by title* — titles grow long, change along the chain (F4), and cannot be
shortened in speech.

**Symptom of violation.** An older pointer leads to a record that plainly
discusses something else.

### F7 · The rejected alternatives are mandatory, with their reasons

**What holds.** Every record names the options that were seriously on the table
and says, for each, why it did not win.

**Why.** Without this part, the next person proposes the rejected solution again
— and nobody remembers the reason, so it is recomputed or, worse, adopted. The
value of a body of decision records does not lie in the decisions that were
taken; those show up in the result anyway. It lies in the ones that were **not**
taken. This holds all the more when assistant systems read along: they reliably
propose the most obvious solution, and the most obvious one is often exactly the
one that was once rejected for good reason.

**Obvious and rejected.** *Record only the chosen option and keep alternatives
in memory* — memory is the least reliable part of the system and the only part
nobody backs up. *List alternatives without reasons* — a list without reasons is
an open invitation to evaluate them all over again.

**Symptom of violation.** In a meeting someone proposes what the body of records
already rejected once, and nobody notices.

### F8 · An index shows the standing of the whole body

**What holds.** Above all records sits an overview showing, for every record,
whether it applies or is superseded and by what. It is updated in the same move
as every new record.

**Why.** The body is read by full-text search, and full-text search knows no
status: it returns the superseded record just like the applicable one, often
first, because the older one has existed longer and is linked more often.
Without an index, the question "what actually applies?" can only be answered by
opening **every** record. Nobody does that.

**Obvious and rejected.** *Rely on the status marker inside the record* —
necessary but not sufficient: it answers the question only once you have already
found the right record. *Generate the index from the heads* — correct and
recommended as soon as a tool exists; without a tool a hand-kept index beats no
index, and the effort is one line per decision.

**Symptom of violation.** Someone implements a decision that has been superseded
for half a year, citing the body of records while doing so.

**Load-bearing structure.** F2 carries everything: if immutability falls, F3,
F4, F5 and F6 lose their purpose — they only govern what a replacement looks
like, and without immutability there would be no replacements, only corrections.
F1 is the precondition of F2: only what applies as a whole can be immutable. F7
and F8 stand independently — they make sense even for an amended body of records
and can be adopted separately. Whoever wants to take away one rule takes F7.

## 5. Security considerations

A documentation method appears security-neutral. It is not: immutability also
covers what got in by accident, and a misread standing can put an abandoned
protective measure back into force.

| Threat | Typical course | Countermeasure |
|---|---|---|
| **A secret becomes immutable** | An example in the record contains a real key, hostname or credential. It is noticed later — but the text may no longer be changed | Check before acceptance; use invented values in examples as a matter of course. If it happens anyway, **secret rotation takes precedence over immutability**: the key is rotated, and removing it from the text is handled as an explicit, logged exception — not as a silent correction |
| **A superseded security decision is implemented** | Someone searches the body for "access" and hits the older, more detailed record first. It describes an open read access that was abandoned long ago | Status marker **inside the record itself**, not only in the index (F5), and visible at the top, not in the head block alone. A reader who only reads the opening must see the supersession |
| **An assistant system pulls in superseded decisions** | A tool reads the whole folder into its context and weighs all records equally | Index with unambiguous marking (F8) plus an entry note saying explicitly that superseded records are kept — otherwise their mere presence reads as validity |
| **Retroactive reinterpretation** | A participant later edits the rationale of a decision that has become inconvenient | Immutability is itself the countermeasure here, provided the body is version-controlled: every change is visible as such |

**What the concept does not protect against:** it does not stop a decision from
being factually wrong, and it is no substitute for access control. A body of
decision records routinely reveals more about a system's architecture than its
source code — whoever publishes it should do so on purpose.

## 6. What it costs

- **More documents.** A body that would have twenty sections when amended has
  forty files here, a third of which no longer apply.
- **One sentence of change costs a whole record.** This is the sore point: the
  effort is noticeable, and the temptation to "just quickly" correct the old
  record is greatest at exactly this spot — with small changes, where the method
  feels most expensive.
- **Redundancy between predecessor and successor.** Large parts get copied over.
  Whoever reads both reads much of it twice.
- **The index is a duty one can forget.** If it is forgotten, the benefit of the
  whole method collapses without anything looking broken.
- **The chain wants reading.** Understanding the prehistory of a decision means
  opening three records instead of one paragraph.

## 7. Scope and limits

**Preconditions.** The body lives as text files in a version-controlled
directory, one file per decision, with a machine-readable head. Nothing more is
required — no tool, no database, no web front end. Whoever writes must be
willing to write a record to the end rather than sketch it.

**Tipping conditions.** The answer becomes wrong when (a) decisions change
**weekly** — then you replace more than you decide, and the body turns into a
chain without substance; (b) the decisions are not decisions at all but external
mandates one merely notes — then the core is missing (F7) and a plain list will
do; (c) a body grows so large that a hand-kept index no longer matches — beyond
that point F8 needs tool support, or the overview itself becomes the error
source.

**Not solved.** This concept says nothing about **what** to build — it replaces
neither a specification nor requirements documentation. Nor does it describe how
a body **comes into being** when decisions were made verbally for years;
reconstructing them afterwards is a separate task with its own traps. And it
does not govern who is allowed to decide.

## 8. Track record

**This mechanism is not new, and that is its best evidence.** It is the core of
what has been practised since 2011 as an *Architecture Decision Record*: the
original description already demands that an accepted record not be changed but
replaced by a new one. Anyone who has worked with ADRs knows the procedure —
usually as a quirk of a format, though, not as a self-standing answer to a
question.

That is precisely why it stands here as a dossier of its own. The question "how
do I keep decisions traceable?" is **not confined to architecture decisions** —
it arises just as much for product, procurement, operational and editorial
decisions, where the mechanism helps while the vocabulary ("architecture
decision record") misleads and the accompanying file format is oversized. What
this dossier contributes is not the invention but the **separation of the
mechanism from the format**: it applies to any decision log, and decisions F1 to
F8 say which parts of it are load-bearing and which are not.

The author's own experience with it comes from a private project with a body of
thirty-seven records, nine of them superseded. It did not start on a green field but arose
from a **migration out of a collection file** — and that migration is the most
solid experience with it, because it exposed the weaknesses of the prior state:
in the collection file, revisions had landed as additional paragraphs beneath
the original decision ("revision of …"), so that several entries were valid and
invalid at the same time, depending on the paragraph.

Two experiences from that migration sharpened decisions in this concept:

- The executing editor found a **wrong total** in the migration plan — the plan
  demanded a record in its prose that its own sum failed to count. Whoever had
  followed the number instead of the text would have silently lost a decision
  and its rationale. Hence the rule for every migration: **when in doubt, keep.**
- A superseded record had initially been given its successor's title so the
  index would look "consistent". The result was two identically named entries
  and ambiguous citations — hence F4.

> **Example** (from the same project, followed through): for the items of an
> inventory system it was decided that their **visible number is an identity
> attribute** — it identifies the item, is unique, and does not change. The
> record was titled "Visible number: identity attribute, not history data",
> rationale: whoever reads a number should find exactly one item with it.
>
> Half a year later it turned out that in practice numbers are assigned
> afterwards and corrected — labels fall off, numbering schemes get reorganized.
> The decision was wrong. What followed was **not an added paragraph** in the
> old record but a new one with its own title ("Visible number: an attribute of
> its own, versioned in the history"), its own complete rationale, and its own
> list of rejected alternatives — including the old decision itself, now with
> the reason it failed.
>
> The head of the new record reads `supersedes: [0011]`, the head of the old one
> `superseded-by: [0031]` and `status: superseded`; in the index the line for
> 0011 is struck through and carries the note "↳ superseded by 0031". Whoever
> searches for the numbering scheme today finds both records — and knows within
> two seconds which one applies and that there was a predecessor given up for a
> good reason.

The status of this dossier stays **Draft**: a body of this size and a few months
of operation are too little for "Proven". In particular, tipping condition (a) —
high change frequency — has not been experienced yet.

## 9. Open questions

**Q1** Beyond what size does a hand-kept index stop working? The author's guess
is around fifty records; this is not substantiated. Experience from larger
bodies is sought.

**Q2** How do you handle a decision that is very long and of which only a small
part changes? F3 demands a complete rewrite, and for a ten-page record that is a
real hurdle — with the risk that the decision therefore goes *undocumented*. Is
the right answer "such records are too big anyway and should have been split",
or is an exception needed here?

**Q3** May there be a record that collects **several** early decisions never
documented individually — as a chronicle at the start of a body? This plainly
violates F1. The author allowed it because the alternative would have been to
invent a dozen decisions retroactively for which no reliable memory existed.
Whether that is a legitimate exception or the first crack is open.

**Q4** How do you keep pointers from source code to decision numbers current
when a decision is superseded? A comment pointing at the old number is not wrong
under F6 — but it does point at the superseded record.

## 10. Sources & related work

- **MADR — Markdown Any Decision Records**, format standard for decision
  records, version 4.0.0: <https://adr.github.io/madr/> (retrieved 2026-08-13).
  This concept uses MADR as its file format and extends it with the two relation
  fields as lists (F5).
- **Michael Nygard, "Documenting Architecture Decisions"** (2011), the original
  description of the method including the principle of not changing accepted
  records but replacing them:
  <https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions>
  (retrieved 2026-08-13).
- **Overview of decision documentation**, a collection of templates and field
  reports: <https://adr.github.io/> (retrieved 2026-08-13).
- **Deepening, not a prerequisite — the concrete form:** the sister dossier
  `decision-records-with-madr`
  (<https://github.com/schlingensiepen/proposals/tree/main/decision-records-with-madr>)
  describes what this mechanism looks like in MADR: which head fields, which
  sections, which file name, how the replacement is entered — with a
  ready-to-copy template. Whoever applies this mechanism to a body of
  architecture or software decisions finds the finished form there. **This
  dossier is fully usable without it** — the mechanism is deliberately described
  independently of any format and holds for any decision log.
