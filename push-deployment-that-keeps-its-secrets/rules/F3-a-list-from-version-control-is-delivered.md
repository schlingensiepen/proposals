---
type: Proposal Rule
description: The package contents are the list of versioned files of a component, not the contents of its directory on the deliverer's machine; protective files in otherwise ignored directories are versioned explicitly.
number: F3
binding: must
role: independent
applies_to: [builder]
---

# F3 · What is delivered is a list from version control

**Applies to:** builder.

**What holds.** The package contents are the list of **versioned** files of a component, not the contents of its directory on the deliverer's machine.

**Why.** The working directory holds test data, one's own configurations, logs and half-finished attempts. Anyone who transfers the directory transfers them along, and does not notice, because nothing breaks on the target system. "What is versioned" is the only boundary that can be checked without maintenance effort: it is already fixed anyway.

**What follows from this and is easily overlooked:** protective files that are in an otherwise ignored directory, the rule that prevents a folder from being listed, the sample configuration, have to be **explicitly versioned as well**. Otherwise they never reach the server, and the directory stands open while the documentation claims the opposite.

**Considered and rejected.** *Transferring the directory and maintaining exceptions*: the exception list goes stale as soon as somebody creates a new test file. *A build step that assembles a package*: one more stage with a state of its own; if the list exists anyway, it is the package.

**How you notice it is violated.** On the target system there is a file that nobody put there.
