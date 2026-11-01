---
type: Proposal Rule
description: Taking the secret in the address knowingly breaks the rule that a secret never travels in an address; five duties follow, from naming the price at the door and never offering this way on the main surface to narrow secrets and keeping addresses out of stores.
number: F11
binding: must
role: load-bearing
applies_to: [builder, operator, consumer]
---

# F11 · The price, named: the secret in the address is a breach, and the main surface does not offer this way

**What holds.** The second surface takes the secret in the address and
thereby **knowingly breaks** two rules that hold for the rest of the
service: that a secret never travels in an address, and that it is
handled in the ranking *environment variable before pass-through file
before context window*. On this surface the secret stands in every
access log of every proxy and every server, in browser histories, in
redirects, in caches and in the caller's transcript, in everything that
keeps addresses. Five duties follow. **(1)** The description of the
surface names the price at the door, before the first call, in full
sentences: *Your token is the first part of every address. It therefore
ends up in access logs, in histories and in the transcript of whoever
calls. The main surface takes it in a header and does not have this
price. Use this surface because your tool leaves you no choice, not
because it is more convenient.* **(2)** The main surface MUST NOT offer
this way: no secret in the path, none in the query, under no name;
otherwise the safe way no longer has precedence, and the breach is not
an exception but a second rule. **(3)** A secret used on this surface is
a **narrowly cut** one: one capability at one place, with a target where
a leak does bounded harm; a secret with broad rights or administrative
power never belongs in an address. **(4)** The operator knows on issuing
that a secret will be used on this surface, and treats it accordingly:
shorter-lived, rotated more readily, revoked without hesitation.
Whoever can, issues a **separate** secret for the second surface rather
than the same one that travels in a header on the main surface. **(5)**
Everything this surface controls keeps addresses out of stores: its
answers tell caches that they must not be kept, and its own logs shorten
the secret segment; for the logs of stations in front, the service's
description gives the operator the sentence that they should do the same
there. Duties (1) and (2) are built in the author's implementation; (3)
follows there from the cut of the tokens; (4) and (5) go beyond it and
are marked as such (see the evidence).

**Why.** The secret in the address is the real price of this surface,
not a bookkeeping item, and it contradicts head-on what the series
otherwise says about secrets. A price this high is paid honestly only if
it is named, at the place where it is incurred, and if the surroundings
are built so that only whoever must pays it: hence the sentence at the
door, hence the ban on the main surface, hence the narrow cut. The ban
on the main surface is not symmetry but the condition under which the
breach stays a breach: if the main surface offered the same way, there
would be no safe way left against which the emergency exit is the worse
one. A separate secret per surface, because a leak otherwise opens both
ways and rotation interrupts the safe way too. And the care for stores,
because a secret in an address lies at every place that keeps addresses,
and the service should at least clean the places it owns.

**Obvious and rejected.** *Name the price in the service's security
considerations and stay silent in the description*: the caller reads the
description, not the security considerations; it must know before it
sends the first call. *Keep the secret short or obscure it ("it is only
a shorthand")*: a shorthand that grants access is a secret; the length
changes nothing about where it lies. *Let the main surface accept the
path way too "for convenience"*: then of the safe way's precedence only
the intention remains. *A secret with more rights, "so that you can do
everything on the emergency exit"*: you can do everything on the
emergency exit that the main surface can anyway
([F1](./F1-an-emergency-exit-not-a-second-api.md)); more rights would be
more damage in the log.

**How you notice it is violated.** A secret used on the second surface
opens something else as well after a leak; or the main surface accepts a
secret from an address; or a caller uses the second surface although it
could use the first, because nobody told it what it costs.
