---
type: Proposal Rule
description: The second surface carries its own prose description under its prefix, readable without a secret, that hands out every identifier it asks the caller to substitute; it is switched off together with discovery and accepted only after a GET-only reviewer has finished a task with it.
number: F12
binding: must
role: load-bearing
applies_to: [builder]
---

# F12 · The description of the surface is the only document its caller has

**What holds.** The second surface carries its **own description**
under its prefix, `<base>/get/v1`, as prose, retrievable without a
secret, rendered with the base URL of the request, delivered in the form
the weakest intended reader accepts (`text/plain` by default). A
machine-readable interface description does not replace it: a caller
that cannot send `POST` usually cannot process a contract document
either. Because this prose is all the caller has, what holds for every
self-description holds here more strictly: every identifier the text
names for substitution is handed out by a call that is described in the
same breath as its source; the scale of the upload, the period of the
staging area, the irreversibility of the depositing call, the absence of
any undo and the two encoding rules are in the text, not in
experience; and nothing in the text points at a document the caller
cannot process. The surface is switched off **together with the open
self-description** (discovery) of the service: where discovery is off,
the second surface also answers *not found*. And it counts as accepted
only when a reviewer that has nothing but its address, a secret and a
task, and that is expressly allowed to send nothing but `GET`, has
completed the task with it and reported its way.

**Why.** The caller of this surface has less than anyone else: no
contract it could load, no second verb it could try, often no tool that
shows it more than the answer text. Whatever it does not find in the
prose, it must guess; and the four things a reviewing agent had to guess
in the first run (the encoding of free text, the form of the
idempotency key, that there is no undo, that a finished file stays
where it is) it guessed all correctly and said so; the next one may
guess differently. The chain of identifiers is drawn so tightly because
the author, reading the finished text, found that of all things the
identifier of the call that takes effect was named twice for
substitution and handed out not once, and because the first test for it
was satisfied as long as the word appeared somewhere in the neighboring
sentence. The coupling to discovery, because both answer the same
question (may a stranger learn what this service can do?), and two
switches for it would be two truths. And the external check with a
reviewer that may only send `GET`, because the builder can always send
`POST` and therefore does not notice where the surface needs it.

**Considered and rejected.** *Include the surface in the main surface's
interface description*: describes it for a reader that does not need it,
in a form its reader cannot read. *Only hint at the prose in the
service's entry page*: the entry page points with one sentence
([F1](./F1-an-emergency-exit-not-a-second-api.md)); the description
itself needs an address of its own that can be remembered and passed on.
*Make the surface switchable on its own*: two switches for one question.
*The external check with an ordinary agent*: it reaches for `POST` as
soon as something sticks, and the surface is checked without having been
used.

**How you notice it is violated.** A reviewer reports what it had to
guess; or a reader asks where an identifier comes from that the text
names for substitution; or the surface answers although the service's
self-description is switched off.
