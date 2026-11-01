---
type: Proposal Rule
description: The second surface never resolves contradictory or repeated input silently; two conceivable inputs become two routes, a multi-valued field takes one value per call and appends, and a call whose meaning would have to be guessed is refused.
number: F9
binding: must
role: independent
applies_to: [builder]
---

# F9 · No guessing: contradictory and repeated input is refused or made impossible

**What holds.** The second surface never resolves contradictory or
repeated input silently. Where two inputs for the same field would be
conceivable (a text as a value or as a file), they are **two routes**,
so that the question cannot be asked. Where a field holds several
values, **one value per call** arrives, and the call appends instead of
replacing. A part of an upload that arrives a second time with
different content is refused ([F5](./F5-the-upload-in-parts.md)). A
repeated value may not swallow another, and a call whose meaning the
service would have to guess gets a refusal that says what was ambiguous.

**Why.** The description of this surface expressly forswears guessing,
and the first version guessed in two places: with a value and a file for
the same field, the service took the file and dropped the text; with two
recipients in the same call, it kept the last one and lost the first
without a word. A reviewing agent found both, and both answered with
success. A caller that takes things literally does not notice such a
thing: it got an answer that says "accepted". The forms in which
ambiguity cannot be expressed at all are superior to the forms in which
it is checked, because they need no check that can be forgotten.

**Obvious and rejected.** *A precedence rule ("file beats text")*: is
exactly the guessing, only written down; the caller that sent both
meant something, and the service does not know what. *The last value
wins*: loses recipients silently. *Forbid contradictions only in the
description*: the caller reads it, the service does not check it, and
the silent case stays silent.

**How you notice it is violated.** A caller sends two inputs and gets
success, and the result contains only one.
