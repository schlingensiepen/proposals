---
type: Proposal Concept
description: The problem of checks and documents judged by the one who built them, the concept of proof from outside (a check seen to go red, a document used by a reader without inside knowledge), and a two-step procedure.
---

# The concept

## Problem

Green is the state an empty check has too. The cases in which a check
tests nothing all look equally harmless: a check for "something was
logged" while another subsystem logs anyway; a check for a name that
occurs again in the following sentence; a check for a rejection that
happens even without the rule under test, only one stage later. All three
looked like protection until the deliberate counter-test. Where a concept
hangs its rules on checks, a gate that cannot fail is a rule that has in
effect been given up without anyone having decided so.

And a document meant to work without inside knowledge cannot be checked
by reading, only by using, and by someone without that knowledge. The
builder reads the description with the very knowledge it is meant to
replace, and so overlooks exactly the gaps it exists for; the developer's
review rounds over their own work share the blind spots of what is under
review, and the worst finding is regularly inside the fix for the
previous error, where the self-reviewer is most certain of being done.

## The concept

The value of a check is not that it is green but that it can go red. That
can be observed and should be observed, once, while writing it: every
check runs once with its safeguard removed and has to fail. The acceptance
of a document meant to work without inside knowledge is done by a reader
who has nothing but what its later readers will have, and a task; they
complete the task and report their path, and their findings count as
bugs, not as opinions. In the implementation this concept comes from, the
substantial findings came from somewhere other than self-review: from a
real target system, from a reader of the finished text, and from agents
with nothing but a URL. With agents, this test is for the first time cheap
enough to run routinely, at the end of every round.

**Load-bearing structure.** Two rules, each standing on its own. They meet
at one point: a check that cannot go red is usually discovered by someone
who did not know the check, which is the reader without inside knowledge.
If F2 is dropped, F1 still holds; only nobody knows any more whether the
rules are being kept.

## The procedure

Not yet complete; two steps so far.

**1 · See every check go red once.** Before you trust a check, whether a
concept demands it or you added it yourself, run it once with its
safeguard removed and watch it fail. If it stays green, sharpen it until it
tests the one statement only the guarded side can satisfy, and start
again. *Result:* for every check, one observed red run. (F1)

**2 · Close every round with an outside run.** At the end of every round
in which the document or the interface changed, an agent with no project
context gets nothing but what any later reader will have (for a service
the URL and, where one is needed, a narrowly scoped secret for a target
where a wrong move costs nothing; for a document the document itself) and
a task in the user's language. Read its report as
a bug list, fix, and run it again after the next change. *Result:* an
accepted version and a bug list. (F2)
