---
type: Proposal Rule
description: Whatever checks the configuration is the code that starts on it; what the check accepts starts, what it rejects does not, and the check shows the effective values, defaults included.
number: F1
binding: must
role: load-bearing
applies_to: [builder]
---

# F1 · The judgment is the start

**What holds.** Whatever checks the configuration is the code that
starts on it. What the check accepts starts; what it rejects does not.
The check shows the effective values, defaults included, the way the
start will use them.

**Why.** A check that does not reach the same decision as the start is
worse than none: it creates trust that breaks in an incident. And a
check that shows only the values somebody set hides the defaults, which
are in effect all the same.

**Considered and rejected.** *A separate schema validation tool*: the second
truth about the configuration; it agrees as long as someone maintains
both. *Checking by a trial start*: binds ports and state and cannot be
repeated while the service runs.

**How you notice it is violated.** The service does not start although
the check was green.
