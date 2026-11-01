---
type: Proposal Rule
description: The thing built on the server over several fetches is called a composition, never a draft; the depositing call is named like the effecting operation elsewhere in the service and never after the call that finishes an upload.
number: F7
binding: must
role: independent
applies_to: [builder]
---

# F7 · The word: composition, not draft; and depositing is not finishing

**What holds.** The thing that arises on the server over several fetches
is called a **composition** and carries its own identifier. "Draft"
names, in the description of this surface, exactly one state, the one
the result may have at the target system, never the thing on the
server. The call that hands a composition to the operation that takes
effect is named the way that operation is named everywhere else in the
service (in the pattern: `deposit`) and MUST NOT be named after the call
that finishes an upload (in the pattern: `finish`): that one closes a
local collection and has no consequence; this one deposits at the target
system and cannot be undone. Unequal consequences carry unequal names.

**Why.** The thing was first called "draft", and a reader consistently
asked two things: whether the depositing call meant "save as draft", and
whether it should not be called "finish" as with the upload. Both
questions showed the same fault: one word stood for two things, for what
is assembled on the server *and* for what lies in the mailbox at the
end, and the call that turns the one into the other read as "save it as
that". The analogy with the upload was rejected *because of* its
closeness, not despite it: equal names for unequal consequences are the
kind of symmetry that deceives, and the description of this surface
depends on the one irreversible call standing out from all others. The
author paid for this renaming across specification, code, tests and
description; the price would have risen with every day the word lived.

**Obvious and rejected.** *Keep "draft" and explain the difference in
the text*: the reader reads the name, not the explanation; it comes too
late and stands too far away in the context window. *Name both
completions the same, "for consistency"*: consistent would be equal
consequence; this would be consistently wrong. *Name the call after the
state at the target ("deposit as draft")*: ties the name to a setting
of the target system that not every configuration has.

**How you notice it is violated.** A first reader asks whether two
things with the same name are the same; or proposes naming the
depositing call after the finishing one; or an agent treats the
depositing call like saving an intermediate state.
