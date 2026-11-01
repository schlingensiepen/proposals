---
type: Proposal Scope
description: Security considerations of an immutable body of records, what the format costs, where it applies, when the answer flips, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

A file format appears security-neutral. It is not, once the records are immutable: whatever is in there stays in there.

| Threat | Typical course | Countermeasure |
|---|---|---|
| **Personal data becomes immutable** | The MADR fields `decision-makers`, `consulted`, `informed` invite entering real names. In an immutable, possibly published body the name then stands permanently, together with a statement about who was responsible for what | Enter roles instead of names (`architecture`, `operations`), or names deliberately and with consent. Uncritical in a one-person body, not in a team |
| **Secret inside an example** | A record justifies a decision using a real key, hostname or path. It is noticed later, but the text may no longer be changed | Check before acceptance; use invented values in examples as a matter of course. The shipped template therefore contains placeholders only |
| **The file name reveals the architecture** | Descriptive titles (F4) make the directory tree readable, for strangers too. A body whose file names read "open-read-access" and "master-key" is a map for an attacker | Decide deliberately whether the body is published; titles describe the question, not the weakness |
| **The head is read as truth** | A tool or assistant system evaluates head fields only and reports a body as current although a record says otherwise in its prose | Keep F3: if the head holds bookkeeping only, it cannot contradict the text. The check "does the head match the text" then does not arise |

**What the format does not protect against:** it makes no decision right, it is no substitute for access control, and it does not prevent someone from writing a record nobody reads.

## What it costs

* **MADR is verbose.** Five sections for a decision one could state in three sentences. For small decisions the format feels like bureaucracy, and the temptation not to write them down at all is real.
* **The head is maintained by hand.** As long as there is no tool, the two relation lists are pure discipline; a forgotten backward pointer goes unnoticed.
* **Duplicating the options (F6) is double work.** Done sloppily, you end up with a list and a paragraph that contradict each other.
* **Four-digit numbers look silly with twelve records.** The benefit shows only in three-digit bodies; the cost is paid from the start.
* **The template needs maintenance.** When a format rule changes, the template is the first file nobody touches.

## Where it applies

### Preconditions

Markdown files in a version-controlled directory, a tool that at least displays YAML frontmatter, and the willingness to maintain the head by hand. No server, no database, no ADR program.

### When the answer flips

The answer becomes wrong when (a) a tool is in use that enforces its own format: then the tool wins, and this dossier at best describes which fields to reproduce inside it; (b) the body grows so large that hand-kept head fields no longer match: beyond that point a check run is needed that holds relations and index against each other; (c) the decisions have no options because they are external mandates: then F5 and F6 are empty shells and a plain list will do.

### What it does not solve

This dossier describes the **form** and says which places of an accepted record may still be changed (F3, F9). It says nothing about **how a revision proceeds in substance**: when to revise rather than clarify, how completely the replacing record must answer the question afresh, and what happens to parts that still hold unchanged. Nor does it govern who may decide, when a decision is worth recording, or how a body is reconstructed after years of verbal decisions.

### Not yet settled

* Whether the head should carry a field for the **question** the record answers, as one line an index could display. That contradicts F3 ("bookkeeping only") but would solve a real problem: titles are short, and an index of forty titles says less than one hopes.
* How one checks the relation lists automatically without introducing an ADR tool. What is wanted is the smallest conceivable check run that holds both directions and the index against each other.
* Whether the four-digit number (F4) is overkill. It costs two characters in every citation and only pays off beyond a hundred records. Experience from large bodies is welcome.
* For multilingual teams: whether a body in the working language with English titles is a viable middle ground, or the half-translated body from F7 under another name.
