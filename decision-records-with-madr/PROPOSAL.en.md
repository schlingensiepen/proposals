---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-13
author: Jörn Schlingensiepen
slug: decision-records-with-madr
language: en — translated from the German original
sync: v1.0 (2026-08-13)
question: "What does a decision record actually look like — which fields, which sections, which file name?"
thesis: "MADR is adopted unchanged and extended in exactly one place: the replacement relation is kept as two lists in the head."
---

# Using MADR — without rebuilding it

**In one sentence:** A decision record follows MADR 4.0.0 unchanged; the only
extension is two list fields in the head recording what this document supersedes
and what superseded it.

**Abstract:** Whoever sets out to document decisions usually invents a format
first and notices after twenty records that it jams in the same places as the
finished one. MADR is that finished format; it costs nothing, everyone knows it,
and a single extension makes it cover revisions cleanly too. The price is a
somewhat verbose document and a head that is maintained by hand.

## 1. Problem

"I understand that I should write my decisions down. But what does the thing
look like? What goes at the top, what belongs in the text, what is the file
called, and how do I later recognize that this decision has been superseded by
another one? I start inventing a format, and by the third record I no longer
know whether I did it the same way in the first."

Two different questions hide behind this and are readily mixed up. One is about
**content**: what must a decision contain so that it still holds in two years?
The other is about **form**: what does the file look like so that a body of a
hundred records stays searchable, sortable and machine-readable. This dossier
answers the formal question — and answers it in a way that does not work against
the other one.

The sore point is precisely the place no widespread format solves satisfactorily:
**replacement**. Decisions get revised; if the format provides only a single
field for that, or none, the information ends up in the prose, where no tool
finds it and no reader sees it reliably.

## 2. The concept

Every decision is a Markdown file following **MADR 4.0.0**, adopted unchanged: a
YAML head with status, date and participants, below it context and problem,
considered options, the decision with its rationale, and the consequences. The
file name starts with a four-digit number followed by a descriptive short title
in lowercase. MADR is extended in **exactly one** place: the head gains two
additional fields, `supersedes` and `superseded-by`, both as **lists**, because a
revision can merge several decisions and one decision can split into several. One
mandatory section is added to the prose, spelling out the rejected alternatives
with their reasons — so the options appear twice: once as a list for skimming,
once as a paragraph with justification. Above all records sits an index as a
table. And a template lives in the body, to be copied rather than rebuilt from
memory.

**These records are subjected to a mechanism that is older than the format and
does not belong to it:** an accepted record is not changed in content, a
revision is a new record replacing the old one, and the old one stays. That is
the actual point of the exercise — the traceability of a sequence of decisions —
and it is at the same time the reason decision records exist at all. This
dossier describes the **form** that mechanism takes in MADR; wherever a decision
here rests on it, it is spelled out (the term "accepted", F3, F9). Its full
justification as a mechanism in its own right — including for decision logs
without MADR and without any architecture context — is a subject of its own;
section 10 names it. **It is not a prerequisite:** what this dossier needs is in
this dossier.

## 3. Terms

- **MADR** — *Markdown Any Decision Records*, a widespread open format for
  decision records; a Markdown file with a YAML head and fixed sections.
- **Head** (frontmatter) — the YAML block between two lines of three hyphens at
  the start of the file. Machine-readable.
- **Status** — the state of a record: draft, accepted, superseded.
- **Accepted** — the state from which the decision applies. With it the
  **content** of the record becomes immutable: statement, rationale, options and
  consequences are not changed again. Exactly two places are excepted — the head
  (F3) and the opening of the document, where the supersession notice goes (F9)
  — plus typographical errors and dead links. *The reason:* only this keeps it
  datable what applied when. If a record from 2024 contains a sentence from
  2026, no statement in it can be placed in time — and therefore no rationale
  either, because a rationale always holds relative to a state of knowledge.
  Whoever wants to revise writes a new record that replaces the old one.
- **Replacement relation** — the link between a superseded record and the one
  replacing it; kept here in both directions.
- **Body** — the entirety of a project's decision records.
- **Index** — the overview file across the body, from which the standing of each
  record follows.
- **Template** — a shipped sample file copied to create a new record (attachment
  of this dossier, see section 8).

## 4. Decisions

### F1 · MADR 4.0.0 is adopted unchanged

**What holds.** The format is MADR version 4.0.0 — Markdown with a YAML head,
sections in the order it prescribes. It is not renamed, not reordered and not
"adapted to our needs".

**Why.** A homegrown format costs three times: inventing it, explaining it, and
migrating away from it when a tool eventually appears that expects a common
format. MADR is common enough that tools and people know it, and lean enough not
to get in the way. Above all it is **finished**: the questions where a homegrown
format jams after twenty records — where do the options go, where the
consequences, what belongs in the head — are already decided there.

**Obvious and rejected.** *A minimal format of one's own ("title, reason,
done")* — faster at the start, but it lacks exactly the fields that get expensive
later: status, date, options. *Nygard's original format without a YAML head* —
historically the root and still viable, but without a machine-readable head no
index can be generated and no status queried. *An ADR tool with its own format
and storage* — binds the body to a program; the records should stay readable and
writable with a text editor.

**Symptom of violation.** Two records in the same body carry different section
headings, and nobody knows which one was the template.

### F2 · The only extension: both replacement directions as lists

**What holds.** The head carries two additional fields, `supersedes` and
`superseded-by`, always as a **list** — even when empty or holding a single
entry.

**Why.** MADR knows a "superseded" status, but the relation itself is not
properly stored in the format; it usually ends up as a sentence in the prose.
There no tool finds it, and on reading you only see it if you read in the right
place. As a head field it is queryable — and **as a list**, because the relation
is n:m: a revision can merge two old decisions, and a grown decision can split
into two. Both occur once a body is a few years old; a single value would force a
white lie in those cases. That it is **both** directions has a practical reason:
whoever searches the body hits the older record first, because it has existed
longer and is linked more often — it must be able to say on its own that it was
superseded.

**Obvious and rejected.** *Only `superseded-by`, resolving the reverse direction
by search* — works while the body is small and fails silently as soon as someone
renames a file. *Keeping the relation only in the index* — then a record picked
up on its own can no longer be judged, and that is exactly how records are read:
individually, out of a search. *Single values instead of lists* — forbids merging
and splitting, and whoever needs them later writes an ad-hoc note into the prose.

**Symptom of violation.** A head carries `superseded-by` as a plain string
instead of a list — or the replacement lives only in the prose.

### F3 · The head is bookkeeping, never content

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

**Symptom of violation.** The decision can be answered from the head alone — or a
head field was changed because the situation changed, not the status.

### F4 · The file name carries number and descriptive title

**What holds.** File names have the form `<nnnn>-<short-title>.md`: four-digit
running number, hyphen, short title in lowercase with hyphens, ASCII only. The
name is not changed after acceptance, and **a number is never reused** — not
even one freed by a discarded draft. Gaps in the sequence are left open.

**Why.** The number sorts and is citable; the title makes the directory listing
readable without opening anything — the whole body becomes its own table of
contents. Four digits, because three-digit numbers break sorting at a thousand
records and the fix would then touch every pointer. **No reuse**, because
numbers get cited — in meetings, in source comments, in other documents. A
reused number makes every earlier citation silently wrong: the pointer still
resolves to an existing record, just the wrong one. A dead pointer is harmless;
a redirected one is dangerous. Lowercase and ASCII so that
file systems and encodings never become the topic — a body travels between
machines, and a non-ASCII character in a file name is the difference between
"works" and "works most of the time".

**Obvious and rejected.** *Number only (`0031.md`)* — shorter, but the directory
listing becomes a column of digits telling nothing. *Title only, without a
number* — sorting becomes alphabetical and thus meaningless, and citations grow
long. *Date instead of number* — two decisions on one day collide, and renaming
to resolve it is forbidden (see above).

**Symptom of violation.** The directory listing sorts alphabetically instead of
chronologically — or a pointer to a file name is dead because someone sharpened
the title.

### F5 · Five mandatory sections in fixed order

**What holds.** Every record has, in this order: **Context and problem** ·
**Considered options** · **Decision** (with rationale and trade-off) ·
**Consequences** · **Rejected and why**. No section is dropped; if one is empty,
it says why.

**Why.** A fixed order makes the body readable crosswise — such records are
rarely read front to back; one jumps to the single piece of information needed
("what was the alternative again?"). That only works if the same information sits
in the same place in every record. And a mandatory section bites exactly where
nobody thinks of it by themselves: **consequences** are almost always forgotten,
because they are uninteresting at the moment of deciding and the only thing that
matters a year later.

**Obvious and rejected.** *Drop sections when there is nothing to say* — a
dropped section looks like a forgotten one; a justified empty one is itself a
statement. *Free structure per decision* — convenient in writing, expensive in
every reading, and after thirty records the body has thirty shapes.

**Symptom of violation.** One has to read a whole record to find the rejected
alternatives.

### F6 · The options appear twice — as a list and as a justification

**What holds.** "Considered options" names the alternatives briefly and marks the
chosen one. "Rejected and why" spells out the reason for each rejected one. This
duplication is deliberate.

**Why.** The two sections serve two different readers. One skims and wants to
know in five seconds what was on the table; the other arrives with a concrete
proposal and wants to know why exactly that one already failed. Pressed into one
section, the first loses the overview and the second the justification — a short
list **and** a spelled-out reason do not fit into the same line. The second
section is the more valuable one: the worth of a body lies not in the decisions
that were taken but in the ones deliberately **not** taken.

**Obvious and rejected.** *Only the list, with reasons in parentheses* —
parentheses enforce brevity, and brevity is the wrong thing for a justification.
*Only the justification section* — then the overview is missing, and one reads
four paragraphs to learn three options.

**Symptom of violation.** An option appears in the list but is missing from
"Rejected and why" — leaving it unclear whether it was examined or forgotten.

### F7 · Head keys in English, prose in one language across the whole body

**What holds.** The keys in the head (`status`, `date`, `decision-makers`,
`supersedes`, …) stay English, as MADR prescribes. The prose is written in one
language, and in the same one across the entire body.

**Why.** The head keys are part of the format, not of the content — translate
them and it is no longer MADR, and every tool reading the head breaks. The prose
addresses people and may use the language in which the decision was taken: a
rationale in a foreign language loses exactly the nuance it was written down for.
What matters is **uniformity** — a half-translated body is worse than either pure
variant, because every search has to be run in two languages.

**Obvious and rejected.** *Everything in English, because software is English* —
right as soon as outside contribution is expected; otherwise one pays in
precision for a benefit nobody redeems. *Bilingual records* — double maintenance
for a document that is not supposed to be touched again after acceptance.

**Symptom of violation.** A full-text search for a technical term misses half the
body.

### F8 · A template lives in the body and is copied

**What holds.** The body contains a sample file with all head fields and all
sections, including embedded notes on what belongs in them. A new record is
created by copying that file — not by rebuilding it from memory or copying the
last record.

**Why.** Without a template everyone takes the most recently written record as a
model — and inherits its idiosyncrasies along with its mistakes. After ten
generations the body has a shape nobody decided on. A template is also the only
place where the format rules can live without anyone having to look for them:
they sit as comments exactly where they are needed. This counts double when
assistant systems write along — they reliably copy what they find.

**Obvious and rejected.** *Put the rules in a guide* — necessary for the why, but
nobody opens a guide while writing. *Take the last record as a model* — precisely
the path along which deviations are inherited.

**Symptom of violation.** A new record has one head field fewer than the
template, and nobody notices.

### F9 · The supersession notice sits at the opening of the superseded record

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

**Symptom of violation.** Someone cites a decision from a record that has been
superseded for months — and has never seen its head.

### F10 · The index is a table showing standing

**What holds.** Above all records sits an overview file: one row per record with
number, title, decision date and relation. Superseded rows are marked as such —
struck through and carrying the pointer to the successor. The index is updated
in the same move as every new record.

**Why.** The body is read by full-text search, and full-text search knows no
status: it returns the superseded record just like the applicable one, often
first. Without an index, the question "what actually applies?" can only be
answered by opening **every** record — nobody does that. This puts the
replacement in three places: in both heads (F2), in the superseded record itself
(F9), and here. That is not waste but the answer to three different access
paths — tool, single opened record, overview.

**Obvious and rejected.** *Rely on the status marker inside the record* —
necessary but not sufficient: it answers the question only once you have already
found the right record. *Generate the index from the heads* — correct and
recommended as soon as a tool exists; without a tool a hand-kept index beats no
index, and the effort is one line per decision. *Rely on the file system's
directory listing* — it shows number and title (F4), but no status.

**Symptom of violation.** Someone implements a decision that has been superseded
for half a year, citing the body of records while doing so.

**Load-bearing structure.** F1 carries F3 and F5 — both are MADR's own shape, not
our invention; whoever gives up F1 must justify them anew. F2, F9 and F10 belong
together: they carry the same replacement in three places, one per access path;
if one falls, the replacement stays true but becomes invisible to part of the
readership. F3 rests on the immutability of the content — without it, what sits
in the head would not matter. F4, F7 and F8 are craft and independent of one
another, each adoptable separately. F6 is a readability decision; dropping it
loses convenience, not substance. **Whoever takes only one rule takes F8** — the
template silently carries all the others.

## 5. Security considerations

A file format appears security-neutral. It is not, once the records are
immutable: whatever is in there stays in there.

| Threat | Typical course | Countermeasure |
|---|---|---|
| **Personal data becomes immutable** | The MADR fields `decision-makers`, `consulted`, `informed` invite entering real names. In an immutable, possibly published body the name then stands permanently — together with a statement about who was responsible for what | Enter roles instead of names (`architecture`, `operations`), or names deliberately and with consent. Uncritical in a one-person body, not in a team |
| **Secret inside an example** | A record justifies a decision using a real key, hostname or path. It is noticed later — but the text may no longer be changed | Check before acceptance; use invented values in examples as a matter of course. The shipped template therefore contains placeholders only |
| **The file name reveals the architecture** | Descriptive titles (F4) make the directory tree readable — for strangers too. A body whose file names read "open-read-access" and "master-key" is a map for an attacker | Decide deliberately whether the body is published; titles describe the question, not the weakness |
| **The head is read as truth** | A tool or assistant system evaluates head fields only and reports a body as current although a record says otherwise in its prose | Keep F3: if the head holds bookkeeping only, it cannot contradict the text. The check "does the head match the text" then does not arise |

**What the format does not protect against:** it makes no decision right, it is
no substitute for access control, and it does not prevent someone from writing a
record nobody reads.

## 6. What it costs

- **MADR is verbose.** Five sections for a decision one could state in three
  sentences. For small decisions the format feels like bureaucracy — and the
  temptation not to write them down at all is real.
- **The head is maintained by hand.** As long as there is no tool, the two
  relation lists are pure discipline; a forgotten backward pointer goes
  unnoticed.
- **Duplicating the options (F6) is double work.** Done sloppily, you end up with
  a list and a paragraph that contradict each other.
- **Four-digit numbers look silly with twelve records.** The benefit shows only in
  three-digit bodies — the cost is paid from the start.
- **The template needs maintenance.** When a format rule changes, the template is
  the first file nobody touches.

## 7. Scope and limits

**Preconditions.** Markdown files in a version-controlled directory, a tool that
at least displays YAML frontmatter, and the willingness to maintain the head by
hand. No server, no database, no ADR program.

**Tipping conditions.** The answer becomes wrong when (a) a tool is in use that
enforces its own format — then the tool wins, and this dossier at best describes
which fields to reproduce inside it; (b) the body grows so large that hand-kept
head fields no longer match — beyond that point a check run is needed that holds
relations and index against each other; (c) the decisions have no options because
they are external mandates — then F5 and F6 are empty shells and a plain list
will do.

**Not solved.** This dossier describes the **form** and says which places of an
accepted record may still be changed (F3, F9). It says nothing about **how a
revision proceeds in substance** — when to revise rather than clarify, how
completely the replacing record must answer the question afresh, and what
happens to parts that still hold unchanged. Nor does it govern who may decide,
when a decision is worth recording, or
how a body is reconstructed after years of verbal decisions.

## 8. Track record

The format runs in a private project of the author with thirty-seven records,
nine of them superseded. It did not start on a green field but arose from a
**migration out of a collection file** in which revisions had landed as extra
paragraphs beneath the original decision. That migration forced two decisions in
this dossier: F2, because three records surfaced that each merged two
predecessors — with a single-valued field one of them would have had to be
concealed; and F8, because the first hand-rebuilt records each turned out to be
missing a different head field.

**Attachment.** The template ships with this dossier — German as
`examples/adr-template.de.md`, English as `examples/adr-template.en.md`. It is
the actual subject of F8: copy, rename, fill in. Its wording, reproduced in full
here so this dossier stays usable without the attachment:

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

> **Example** (from the same project, followed through): for the items of an
> inventory system it was decided that their visible number is an identity
> attribute. The record was called
> `0011-sichtbare-nummer-identitaetsattribut.md`, its head carried
> `status: accepted`, `date: 2026-08-07`, both relation lists empty.
>
> Half a year later it turned out that numbers are assigned afterwards and
> corrected. `0031-sichtbare-nummer-versioniert.md` came into being with
> `supersedes: [0011]`; in `0011`, `status` was set to superseded,
> `superseded-by: [0031]` was added, and a notice pointing at the successor was
> placed above the heading. Nothing in the prose of `0011` was changed — it still
> describes the decision of 2026-08-07, including the rationale that seemed right
> at the time.
>
> In the index the line for `0011` is struck through with the note "↳ superseded
> by 0031", the line for `0031` with "supersedes 0011". Both directions are thus
> kept in three places: in both heads and in the index.

The status of this dossier stays **Draft**: a body of this size and a few months
of operation are too little for "Proven". In particular, tipping condition (b) —
hand-kept head fields no longer matching — has not been experienced yet.

## 9. Open questions

**Q1** Should the head carry a field for the **question** the record answers — as
one line an index could display? That contradicts F3 ("bookkeeping only") but
would solve a real problem: titles are short, and an index of forty titles says
less than one hopes.

**Q2** How does one check the relation lists automatically without introducing an
ADR tool? What is wanted is the smallest conceivable check run that holds both
directions and the index against each other.

**Q3** Is the four-digit number (F4) overkill? It costs two characters in every
citation and only pays off beyond a hundred records. Experience from large bodies
is welcome.

**Q4** For multilingual teams: is a body in the working language with English
titles a viable middle ground, or is that the half-translated body from F7 under
another name?

## 10. Sources & related work

- **MADR — Markdown Any Decision Records**, version 4.0.0, with templates and
  field descriptions: <https://adr.github.io/madr/> (retrieved 2026-08-13).
- **Michael Nygard, "Documenting Architecture Decisions"** (2011), the original
  format without a YAML head from which MADR grew:
  <https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions>
  (retrieved 2026-08-13).
- **Collection of templates and field reports on decision documentation:**
  <https://adr.github.io/> (retrieved 2026-08-13).
- **Deepening, not a prerequisite — and the origin of the rules in section 2:**
  the sister dossier `immutable-decision-records`
  (<https://github.com/schlingensiepen/proposals/tree/main/immutable-decision-records>)
  describes the mechanism the
  records here are subjected to — one record per decision, immutability after
  acceptance, complete replacement instead of correction — and does so
  **detached from the format**: it holds for any decision log, without MADR and
  without any architecture context. It also covers what this dossier
  deliberately leaves open: how a revision proceeds in substance, why a
  replacement must be complete, and what a superseded record contributes to the
  body. **This dossier is fully usable without it** — the rules its own
  decisions rest on are spelled out here, with their reasons.
