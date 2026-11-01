---
type: Proposal Rule
description: Above the individual scripts stands one entry point with three modes, run targets in the enforced order, create a target, list what exists; the sample configuration belongs to each script, not to the entry point.
number: F8
binding: must
role: independent
applies_to: [builder, operator]
---

# F8 · One entry point that runs, creates and lists

**Applies to:** builder, operator.

**What holds.** Above the individual scripts stands an entry point with three modes: **run** targets (in the order enforced by F7), **create** a target (directory with narrow permissions, configuration template copied in, nothing transferred), and **list** what exists.

**Why.** The instance directories are ignored by version control; they appear neither in the repository nor in the documentation. Whoever comes back after months knows neither what their targets are called nor in which order they come. A tool can answer both, and then nobody has to remember.

**The template belongs to the script, not to the entry point.** Beside each delivery script lies its versioned sample configuration; the entry point copies it and does not know its contents. Otherwise the list of keys would stand in two places and drift apart as soon as a script needs a new one.

**Obvious and rejected.** *Only the individual scripts, with order and names in the guide*: shifts three questions into a document that nobody has open while delivering. *The entry point asks for the values interactively when creating*: a second configuration path with a check of its own and the question of what is left behind on an abort; a commented template does the same and is still readable in half a year.

**How you notice it is violated.** Somebody has to look up in the documentation what their own targets are called.
