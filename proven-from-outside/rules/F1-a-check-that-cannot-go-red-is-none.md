---
type: Proposal Rule
description: Every check, whether a concept demands it or a builder adds it, is run once with its safeguard removed while it is written and must fail; if it stays green, it is sharpened until it tests the one statement only the guarded side can satisfy.
number: F1
binding: must
role: independent
applies_to: [builder]
---

# F1 · A check that cannot go red is no check

**What holds.** Every check a concept demands, and every check a builder
adds beyond that, is run **once with its safeguard removed** while it is
being written, and MUST fail. If it stays green, it is sharpened until it
tests exactly the statement that only the guarded side can satisfy, and
not one that happens to be true without the rule.

**Why.** The value of a check is not that it is green but that it can go
red. That can be observed and should be observed, once, while writing it.
The cases that make this necessary all look equally harmless: a check for
"something was logged" while another subsystem logs anyway; a check for a
name that occurs again in the following sentence; a check for a rejection
that happens even without the rule under test, only one stage later. All
three looked like protection until the deliberate counter-test. For a
concept that explicitly hangs its rules on checks, this counts double: a
gate that cannot fail is a rule that has in effect been given up without
anyone having decided so.

**Obvious and rejected.** *Believing the check because it is green*: green
is the state an empty check has too. *Test coverage as the measure*: it
measures which lines ran, not whether a statement was tested. *The
counter-test only for "critical" checks*: the three observed cases were all
uncritical, until they were not.

**How you notice it is violated.** A rule is violated, and the check that
was meant to hold it is green; usually discovered by someone who did not
know the check ([F2](./F2-acceptance-by-a-reader-without-inside-knowledge.md)).
