---
type: Proposal Rule
description: A new version migrates data of an older structure in the service mode at start, before it listens, and records the structure version; a rollback finds converted data and says so.
number: F13
binding: must
role: independent
applies_to: [builder, operator]
---

# F13 · Data migration runs at start

**Applies to:** builder · operator.

**What holds.** If a new version changes the structure of the store or of other operating data, the service mode converts the existing data at start, before it listens: it recognizes the old structure, migrates it, records the new structure version in the state directory and reports what it did. The structure version is recorded from the first start on, so that every later version can tell what it finds. There is no separate command for it. A rollback to an older version therefore finds converted data; that version reports at start that it cannot read the structure, and does not start.

**Why.** The cut aims at deployments where nobody stands at the target (one deliverable, one deployment file, F4): a command somebody would have to run before the start is forgotten there, and a version that cannot work with the old data then does not run at all. The automatic way makes the migration impossible to forget; its price, the rollback, is named and recognizable through the structure version instead of silent.

**Obvious and rejected.** *A migration command, invoked explicitly*: makes the migration a decision and fits F8, but is forgotten in automated deployments; whoever wants it anyway builds it as an additional mode and still lets the start run it. *Backward migration on rollback*: doubles the conversion code for a rare case; instead, a backup before the version change.

**How you notice it is violated.** A new version starts and keeps working with data of the old structure; or an operator has to run a command before the start that they only know about from the changelog.
