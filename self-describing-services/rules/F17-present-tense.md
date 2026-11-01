---
type: Proposal Rule
description: The delivered description names only forms the service knows now; hindsight belongs in the decision records that travel with the source code.
number: F17
binding: must
role: load-bearing
applies_to: [builder]
---

# F17 · Present tense: the description says what is, not what was

**Applies to:** builder.

**What holds.** The delivered description names exclusively forms the service knows *now*. Hindsight ("this path has been removed", "formerly this was called …", decision numbers and decision history, error examples left standing for removed mechanisms) belongs in the decision records that travel with the source code, not in entry page, usage description or interface description. A check in the build keeps patterns of hindsight out of every delivered description.

**Why.** A document read as an instruction knows no difference between "this exists" and "this existed": whoever names a removed form supplies a form to imitate. For the builder, the sentence "removed with that decision" is honest history; for the agent that has nothing but this text, it is the naming of a form the service apparently knows, and he uses it, in the observed case with a real failed attempt against a third-party system that answers repeated failed attempts with a lockout. Honesty about the past has its place; the interface description is not it.

**Obvious and rejected.** *Leaving hindsight as a footnote or warning box*: the agent reads footnotes as text; the warning is prose about it, the example is the instruction. *"Deprecated" markers on old forms*: a marked form is still a named one; what is marked is what should stay, not what is gone. *Deleting the history entirely*: no; it moves to where it belongs, into the decision records that accompany the source code; there whoever builds reads it, and nobody who only calls.

**How you notice it is violated.** A consumer uses a form the service no longer knows, and the search for the fault finds it verbatim in the current description, with the addition that it no longer exists.
