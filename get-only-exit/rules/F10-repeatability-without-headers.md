---
type: Proposal Rule
description: The idempotency key travels as the last path segment of the depositing call and reaches the same operation as on the main surface; the receipt stays on the composition, so a repeat answers the same receipt whatever key it names, and a key already used for another composition is refused.
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
main surface: for the same composition, the same key with the same
secret produces no second effect, but the same identifier. The key MUST be path-safe (letters,
digits, `-`, `_`, `.`), and the description says that you choose a fresh
one per intended effect, not per attempt. In addition, the **receipt**
of the depositing call stays on the composition: every further
depositing call on the same composition answers with the same receipt
and deposits nothing a second time, **regardless of which key it
names**, because the receipt is on the object and not on the key,
until the staging area sweeps the composition after its fixed period.
The two safeguards thus act at different places, and the builder MUST
say so in the description: the receipt protects **one** composition
from being deposited twice; the key protects against depositing
**different** compositions twice. The key is therefore never resolved
silently ([F9](./F9-no-guessing.md)): the same key with the same
composition answers the same receipt and takes no second effect; the
same key with a **different** composition is refused, and the refusal
names the key and the composition it belongs to, in the form of
[F8](./F8-refusals-that-work-on-this-surface.md). For that the service
remembers, per key, the composition it was used for, which it keeps for
the receipt anyway. For parts of an upload the simpler rule of
[F5](./F5-the-upload-in-parts.md) applies: the repeated part has no
consequence.

**Why.** This surface exists for callers whose tool is weak, and a weak
tool does not always see answers. A caller that had a dropped connection must
be able to send the same call again without the effect occurring a
second time, and must *learn* afterwards whether it occurred. The key in
the path achieves the first at the underlying operation, where the main
surface achieves it too; the receipt on the composition achieves the
second, because a `404` ("no longer exists, was deposited and swept")
would be the one answer that tells the caller nothing about whether its
request took effect. And the key is the caller's choice, not a random
number of the service, because a caller that did not see the answer
would not have seen a number assigned by the service either.

**Considered and rejected.** *No protection against repetition ("it is only
an emergency exit")*: the emergency exit is exactly the way on which
repetitions are most frequent. *The key as a query parameter*:
[F2](./F2-everything-in-the-path-the-secret-first.md). *Delete the
composition on depositing and rely on the key alone*: the key helps the
underlying operation, but the caller gets "not found" and knows nothing
([F6](./F6-the-composition.md)). *Take the composition's identifier as
the key*: then the composition can be deposited only once, but a caller
that starts over with the same request after the sweep has no
protection any more. *Let a used key quietly answer the old identifier*:
the first form of this rule; it resolves a contradiction silently, which
[F9](./F9-no-guessing.md) forbids, and the caller learns of the missing
message only at the target.

**How you notice it is violated.** After a dropped connection there are
two messages at the target; or the second call answers "not found", and
the caller asks its user whether to try again. And the silent form that
asks nobody: a caller deposits two different compositions one after the
other under the same key, the second answers with success and the old
identifier instead of a refusal, and the caller only notices from the
missing item at the target that the second never came into being.
