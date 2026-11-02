---
type: Proposal Rule
description: If the available tools cannot write submodule references, the separation is built as two independent roots, one repository in an ignored subdirectory of the other, with the connection written out in the entry-point documents; the alternative to F4.
number: F5
binding: must
role: independent
applies_to: [author]
---

# F5 · Two-roots form: the frugal form, when only files can be written

**Applies to:** author. Alternative to F4; the tool situation decides which one applies.

**What holds.** If the available tools cannot write submodule references, the separation is implemented as two independent roots: one repository is in a subdirectory of the other and is on the ignore list there. There is then no recorded reference; the fact that the two belong together is written out in the entry-point documents of both sides.

**Why.** This form is not a makeshift born of convenience but the answer to a hard tool limit that occurred in practice. A submodule reference is not file content but an entry type of its own in the directory structure, and interfaces that edit repositories through file operations can create and change files but cannot write exactly this entry type. Anyone who works exclusively through such an interface can neither create a submodule nor advance its reference. Then the frugal form is more honest than a submodule that is never updated and whose reference grows less accurate over time.

**Considered and rejected.** *Creating the submodule anyway and maintaining the reference by hand*: it goes stale, and a stale reference is worse than none because it pretends to a precision it does not have. *Giving up the separation because the connection cannot be made cleanly*: throws away F1 to save F4; the separation holds even without a reference. *Copying the product into the work repository and publishing from there*: creates two truths about the same product that drift apart. *Switching to another tool*: legitimate, but often not in the hands of the person doing the work; the rule is meant to hold then too.

**How you notice it is violated.** Nobody can say which state of the product belongs to a given working state, and nobody misses it. (That is at the same time the sign that F4 is missing, not that F5 is wrong.)
