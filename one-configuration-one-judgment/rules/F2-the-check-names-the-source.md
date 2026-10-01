---
type: Proposal Rule
description: Every command says which file it used; with per-value precedence the check names, for every value, the place it came from and marks defaults as such.
number: F2
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F2 · The check names the source

**What holds.** Every command shows which file it used, or that none
was found. With per-value
precedence ([W1 b](../choices/W1-precedence-file-or-value.md)) the check
shows, for every value, the place it came from, and marks defaults as
such.

**Why.** The duty to report sounds like a detail and is the most common
time sink in troubleshooting: two configuration files on one machine,
and you reliably edit the wrong one; or a variable nobody sees any more
overrides the value from the file.

**Obvious and rejected.** *Showing only the file path while variables
are in effect beside it*: the operator reads the file and is misled.

**How you notice it is violated.** Someone spends a long time editing a
file that is not being read.
