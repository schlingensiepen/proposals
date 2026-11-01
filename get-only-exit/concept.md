---
type: Proposal Concept
description: The problem of a self-describing service that a GET-only tool can read but not call, the concept of a second, deliberately restricted surface with its price at the door, and the twelve-step procedure for building it.
---

# The concept

## Problem

The author asked this question as a **user**, after the other one had
already been answered: the service describes itself, the agent has read
the description, the secret is ready, and the first call does not
happen. The tool the agent uses to reach the network can fetch an
address and nothing else: no `POST`, no header of its own, no request
body, often not even a preferred media type. Such tools are not the
exception. They are the link fetchers in chat interfaces, the web-fetch
functions in locked-down sandboxes, the "fetch URL" blocks of automation
platforms, the preview and import functions that take an address and
return its answer. They are built to *read* pages, and that is exactly
why they are allowed everywhere everything else is blocked.

For such a caller a self-describing service is a shop window. It sees
the entry page, the usage description, the interface description, all
open and reachable by `GET`, as the construction principle demands. And
it sees that the reading half of the service consists of `GET` calls as
well. What separates it is a single line: the header in which the secret
travels. A service an agent cannot call excludes it just as much as one
that does not describe itself; the description made the promise, and the
call does not keep it.

Three roles carry the problem. **The user** named the service and wants
their agent to use it, with the tool that agent happens to have, not
with one it would first have to install; doing away with installation was
the whole point of self-description. **The operator** does not want to
spoil the real interface: a secret that may be appended to any address
as a query parameter, and calls that write by `GET`, would color the
whole API, and nobody could say any more which properties it actually
has. **The agent**, finally, believes the description literally and
reports "technical failure" to its user without being able to say why;
it did everything right that it can do, and that was not enough.

The obvious ways out are all bad. *Leave it and mention it in the
guide*: then the description carries a sentence that shows a whole type
of tool the door. *Extend the existing API with `GET` variants*: then the
secret suddenly stands at every address of the main surface, and calls
that take effect by `GET` mix with the reading ones. *Put a small proxy
in front that adds the headers*: that is again something that has to be
installed, exactly what self-description did away with. The structural
answer is a second surface under its own prefix, explicitly named as a
detour: it admits what it gives up, and it gives it up only there.

## The concept

The service keeps its main surface unchanged and puts a **second
surface** beside it under its own prefix, on which every call is a `GET`
without headers of its own. This surface implements nothing itself: it
is a **pass-through** to the same operations the main surface offers,
and it offers no capability the main surface lacks. **Everything is in
the path, nothing in the query**: the secret is the first path segment
after the prefix, on every call, followed by what the call concerns.
**Reading** mirrors the main surface one to one: the same objects, the
same identifiers, the same facts; only the form of the answer follows
what the caller can accept. **Writing without a request body** breaks
every operation that takes effect into small fetches: a file arrives in
numbered parts and is handed to the same upload the main surface uses
only on finishing, so the store never sees half a part; a request with
several parts is filled field by field as a **composition**, can be
shown at any time, and is deposited by **exactly one** call. Every
refusal names a way that *this* surface has, and nothing that is
impossible here. And the surface states its **price** at the door: the
secret stands in the address and therefore in logs, histories and
transcripts. You use this surface because your tool leaves you no
choice, not because it is more convenient.

### What it assumes and what it touches

The concept assumes a service that describes itself: an entry page that
lists the second surface in one sentence, a description the weakest
intended reader can accept, refusals that teach, one word for one thing.
It applies that to a surface whose reader can do even less than the
reader such a service already provides for. `self-describing-services`
treats self-description in depth; this bundle can be understood and
followed without it.

It **breaks**, at exactly one place, the rule shared by several concepts
of this series that a secret never travels in an address, a rule they
ground in the places where addresses are kept. This surface does exactly
that, knowingly, and this concept names the breach together with the
conditions under which it can be justified
([F11](./rules/F11-the-price-named.md)). And it **stands beside**
`service-as-single-artifact`, because a second surface is also a
question of cut: it is not a second function and not a second service
but the same flow in smaller steps, one more pass-through to operations
that exist exactly once (imported from `one-operation-many-surfaces`).

**Load-bearing structure.** [F1](./rules/F1-an-emergency-exit-not-a-second-api.md)
carries everything: drop "emergency exit, not a second API", and F2 to
F10 become construction rules for a main surface with the secret in the
address, and F11 no longer has a safe way against which it would be a
breach. F2 and F3 are the precondition for the surface reaching its
callers at all: with a query parameter or a percent-encoded slash it
fails on the very tools it exists for. F4 keeps the surface a
pass-through on the reading side, F5 and F6 on the writing side; drop F5
and the store sees half files, drop F6 and more than one call takes
effect, and the user's consent has no place any more. F7 hangs on F5 and
F6: two completions with unequal consequences need unequal names,
otherwise the description deceives. F8, F9 and F10 are the conditions
for a weak tool using the surface *to the end*: with an error text that
recommends something impossible, a guess that reports success, or a
repetition that takes effect twice, it arrives but does not get through.
F11 is the rule without which you may not build any of it; drop it, and
the surface is no longer an emergency exit but an open window, and the
series would contradict its own rule on secrets. F12 makes everything
else visible to the caller in the first place; drop it, and every other
rule holds only for whoever has the source code.

## The procedure

1. **Confirm the need.** Name the concrete tools your callers use that
   can send only `GET` without headers. If they can set an
   `Authorization` header after all, stop: the second surface would cost
   its price for nothing. Check the conditions under which the answer
   flips (see the scope) before going on. (F1)
2. **Choose the prefix** and write the one sentence for the entry page.
   (F1)
3. **Make every operation of the main surface a callable function**
   apart from its HTTP handler; the second surface will call those.
   Write the structural test before the first handler. (F1)
4. **Lay out the path shape:** prefix, secret, thing, action, value.
   Decide per segment whether it travels readable or as base64url; list
   the segments that could contain a slash and route them greedily.
   (F2, F3)
5. **Build the reading calls** as mirrors with the form negotiated by
   `Accept`, text by default, `Vary: Accept`, refusals in the one error
   format. (F4)
6. **Build the staging area**, or reuse the service's existing one:
   parts and compositions live there, are swept after a fixed period,
   never reach the store. (F5, F6)
7. **Build the upload in parts:** open, part by number, finish, then the
   same upload function. (F5)
8. **Build the composition:** create, one route per field, show, deposit
   with a key, then the same depositing function; keep the receipt on
   the composition. Have a reader without inside knowledge check the
   vocabulary before the names are in the code. (F6, F7, F10)
9. **Write the refusals for this surface**, then the test that scans
   them for the other surface's words and checks their address form.
   (F8, F9)
10. **Write the prose description:** price first, then encoding, then
    the form of answers, then the calls, each answer-giving call
    introducing the identifier it hands out. Add the tests for the chain
    of identifiers and for the sentences a caller would otherwise have
    to guess. Couple the surface to the discovery switch. (F11, F12)
11. **Decide the secret policy with the operator:** a separate secret
    per surface where possible, shorter life, known at issue; answers
    marked not to be stored by caches; the secret segment shortened in
    the service's own logs. (F11)
12. **Run the external check** with a reviewer that may only send `GET`;
    treat every finding as a defect; revoke the test secret afterwards.
    Repeat after every change. (F12)

The address forms in this bundle (`get/v1`, `compose`, `deposit`,
`finish`) are the author's choice and patterns, not a house convention.
Everything is interchangeable except the shape: the secret first, then
the thing, then the action, then the value.
