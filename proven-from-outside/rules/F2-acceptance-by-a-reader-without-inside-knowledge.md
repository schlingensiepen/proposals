---
type: Proposal Rule
description: A document meant to work without inside knowledge counts as accepted only when an agent with nothing but what any later reader will have and a task in the user's language completes the task and reports its path; at the end of every round, findings treated as bugs.
number: F2
binding: must
role: independent
applies_to: [builder]
---

# F2 · Acceptance by a reader without inside knowledge

**What holds.** A document that is meant to work without inside knowledge
counts as accepted only when an agent that has **nothing but what any
later reader will have, and a task in the user's language** (for a
service the URL and, where one is needed, a secret; for a document the
document itself; no source code, no specification, no conversation with
the builder) completes the task with it and reports its path. This
outside test comes at the end of every round in which the document or the
interface has changed; its findings are treated as bug reports, not as
opinions. Where the outside test needs a secret, it is a narrowly scoped
one, for a target where a wrong move costs nothing.

**Why.** A document meant to work without inside knowledge cannot be
checked by reading, only by using, and by someone who does not have the
inside knowledge. The builder reads the document with the very knowledge
it is meant to replace, and so overlooks exactly the gaps it exists for;
the developer's review rounds over their own work share the blind spots
of what is under review, and the worst finding is regularly inside the
fix for the previous error, where the self-reviewer is most certain of
being done. The substantial findings came from elsewhere: from a real
target system, from a reader of the finished text, and from agents with
nothing but a URL. With agents, this test is for the first time cheap
enough to run routinely: a run takes minutes; the first one against a
fresh surface brought six findings, among them a server error on
malformed input.

**Considered and rejected.** *The builder plays the stranger*: they
cannot forget what they know. *A colleague with project context*: has the
same inside knowledge in diluted form; fewer things trip them up. *Only
unit and integration tests*: they check whether what was built does what
the builder means; the outside test checks whether the document is enough
to want it and to do it right. *The outside test once at the end*: the
findings come with every change; a document that was enough once is not
enough by itself after the next rework.

**How you notice it is violated.** The first readers from outside report
gaps that any outside tester would have found in minutes; or the builder
cannot say when someone without inside knowledge last completed a task
with the document alone.
