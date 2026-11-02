---
type: Proposal Rule
description: The check binds no port and writes no state; it can be repeated at any time beside the running service.
number: F4
binding: must
role: independent
applies_to: [builder]
---

# F4 · Check without binding

**What holds.** The check binds no port and writes no state. It can be
repeated at will alongside the running service.

**Why.** A check that can only run with the service stopped does not
get run.

**Considered and rejected.** *"Just start it and see"*: binds the port the
running service holds and then reports the wrong error.

**How you notice it is violated.** The check fails because the service
is running.
