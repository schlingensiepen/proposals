---
type: Proposal Rule
description: Every refusal of the second surface names a way this surface has; no header, no other verb, no main-surface call, no address form from an earlier shape; a test in the build ensures it.
number: F8
binding: must
role: independent
applies_to: [builder]
---

# F8 · Refusals that work on this surface

**What holds.** Every refusal of the second surface names a way that
**this** surface has. A message that recommends a header, another verb,
a call of the main surface or an idempotency key in a header is
forbidden on this surface, even where it is correct on the main surface
and already exists there. The *unauthorized* refusal names the path form
of the secret; *not found* names the call of this surface that hands out
the identifier sought; an unknown action on a composition lists the
actions there are, instead of pointing at a contract document the caller
cannot process. Where one text must hold for both surfaces, it names the
action instead of the path: "the call that deposits a message" instead
of the concrete address and method. A test in the build ensures, for the
refusals of this surface, that they do not contain the words of the
other surface.

**Why.** An error message is self-description at the moment of failure,
that is, at the moment it is needed most; and a pointer to something
the recipient cannot do is worse than none: it costs an attempt and some
trust, and it makes the description's promise, that every refusal says
what to do instead, false as a whole. Exactly that happened: the second
surface took over the error texts of the first word for word, and they
recommended a header, another call and a repeated header, the three
things whose absence is why the second surface exists. A reviewing
agent that had nothing but the address and a secret called it the most
serious finding of its run. The test in the build belongs to it,
because taking over the finished texts is the cheapest solution and
therefore suggests itself again with every new error path.

**Considered and rejected.** *Take over the main surface's error texts*:
saves editing and gives the recipient, as a remedy, exactly what the
second surface was not built for. *Generic refusals without a way*:
better than the wrong way, but the caller then repeats the same call,
because nothing was taught to it. *Point at the main surface's interface
description*: a caller that cannot send `POST` usually cannot process a
contract document either.

**How you notice it is violated.** A refusal of this surface contains
`POST`, `Authorization` or the name of a header; or a caller, after a
refusal, attempts a call it cannot send at all. And the form that
survives every test because the test asks for the name of the call and
not its shape: a refusal names a way that exists on this surface, but in
an address form it no longer has. It is left over from an earlier shape
of the surface, it recommends something right but spelled wrong, and
the caller follows it literally.
