---
type: Proposal Rule
description: An array in the body and a request without an id are rejected, although the standard permits both.
number: F3
binding: must
role: independent
applies_to: [builder]
---

# F3 · No batch requests, no notifications

**Applies to:** builder.

**What holds.** An array in the body is rejected, as is a request without an
`id`. The standard explicitly permits both; neither is used here.

**Why.** A batch request has no overall outcome: it can partly succeed, so every
error handler and every test needs a second branch that does not exist for a
single call. A notification (a request without a response) moves the question
"did it arrive?" into the caller's confidence. Both buy round trips — and round
trips are not the problem a system with one operator and a handful of clients
has.

**Considered and rejected.** *Leave batch available for the bulk case* — then it
exists, so it gets used, so it must be tested; a capability you do not exclude
is one you maintain. *Notifications for logging* — tempting, but a lost logging
call goes unnoticed, and that is exactly why it is the wrong place to economize.

**How you notice it is violated.** A bug report says "half of it went through" — and
nobody can say which half.
