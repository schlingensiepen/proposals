---
type: Proposal Rule
description: The idempotency key travels as the last path segment of the depositing call and reaches the same operation as on the main surface; the receipt stays on the composition, so a repeat answers the same receipt whatever key it names.
number: F10
binding: must
role: independent
applies_to: [builder]
---

# F10 · Repeatability without headers: the key in the path, the receipt on the composition

**What holds.** Without a header there is no idempotency key in a
header; the second surface therefore carries it as the **last path
segment of the depositing call**, `…/compose/<id>/deposit/<key>`, and
passes it to the same operation that receives it from the header on the
main surface: the same key with the same secret produces no second
effect, but the same identifier. The key MUST be path-safe (letters,
digits, `-`, `_`, `.`), and the description says that you choose a fresh
one per intended effect, not per attempt. In addition, the **receipt**
of the depositing call stays on the composition: every further
depositing call on the same composition answers with the same receipt
and deposits nothing a second time, **regardless of which key it
names**, because the receipt lies on the object and not on the key,
until the staging area sweeps the composition after its fixed period.
The two safeguards thus act at different places, and the builder MUST
say so in the description: the receipt protects **one** composition
from being deposited twice; the key protects against depositing
**different** compositions twice. From this follows the rule for the
caller in its sharp form: a key it has already used once silently robs
a new composition of its effect; it gets the old identifier back, and no
second message arises. For parts of an upload the simpler rule of
[F5](./F5-the-upload-in-parts.md) applies: the repeated part has no
consequence.

**Why.** This surface exists for callers whose tool is weak, and a weak
tool does not always see answers. Whoever had a dropped connection must
be able to send the same call again without the effect occurring a
second time; and must *learn* afterwards whether it occurred. The key in
the path achieves the first at the underlying operation, where the main
surface achieves it too; the receipt on the composition achieves the
second, because a `404` ("no longer exists, was deposited and swept")
would be the one answer that tells the caller nothing about whether its
request took effect. And the key is the caller's choice, not a random
number of the service, because a caller that did not see the answer
would not have seen a number assigned by the service either.

**Obvious and rejected.** *No protection against repetition ("it is only
an emergency exit")*: the emergency exit is exactly the way on which
repetitions are most frequent. *The key as a query parameter*:
[F2](./F2-everything-in-the-path-the-secret-first.md). *Delete the
composition on depositing and rely on the key alone*: the key helps the
underlying operation, but the caller gets "not found" and knows nothing
([F6](./F6-the-composition.md)). *Take the composition's identifier as
the key*: then the composition can be deposited only once, but a caller
that starts over with the same request after the sweep has no
protection any more.

**How you notice it is violated.** After a dropped connection there are
two messages at the target; or the second call answers "not found", and
the caller asks its user whether to try again. And the silent form that
asks nobody: a caller deposits two different requests one after the
other, gets the same identifier twice, and only notices from the missing
item at the target that the second never came into being, because it
used the same key a second time.
