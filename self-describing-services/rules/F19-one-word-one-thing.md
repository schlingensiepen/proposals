---
type: Proposal Rule
description: Every word in the vocabulary of the description denotes one thing; unequal consequences carry unequal names.
number: F19
binding: must
role: load-bearing
applies_to: [builder]
---

# F19 · One word means one thing

**Applies to:** builder.

**What holds.** In the vocabulary of the description (in names of operations, fields, states and components) every word denotes exactly one thing, and unequal consequences carry unequal names: a call that triggers something irreversible is not named like one that closes a local collection without consequence, even if both "finish something". Before a surface is published, somebody without inside knowledge reads it for double meanings; every comprehension question of a first reader counts as a finding, not as a misunderstanding.

**Why.** A double meaning does not strike the author, because he knows both meanings; it strikes the first reader, and as a comprehension question, not as an error message, that is, at no place a gate reaches. An agent that understands an operation from its name builds a wrong expectation from an ambiguous name: "save it as that" instead of "file it irreversibly". And symmetry in names promises symmetry in consequences; where the consequences are unequal, the equal name is the kind of symmetry that deceives. The description depends on the one irreversible call standing out from all others.

**Obvious and rejected.** *Keeping the word and explaining the second meaning in the text*: the reader reads the name, not the explanation; in doubt it comes too late and stands too far away in the context window. *Naming analogous calls alike "so that it is consistent"*: consistent would be equal consequence; an equal name with unequal consequence is a trap with a good conscience. *Postponing the renaming*: the price grows with every day the word lives in specification, code, tests and description (see the costs in the scope).

**How you notice it is violated.** A first reader asks whether two things with the same name are the same, or suggests naming a call "consistently" after another one whose consequence is entirely different.
