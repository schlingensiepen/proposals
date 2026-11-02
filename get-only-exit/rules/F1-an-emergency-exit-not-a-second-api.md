---
type: Proposal Rule
description: A second surface for GET-only callers is allowed only as a named detour under its own prefix, pointed to from the entry page, and as a pass-through that implements nothing and offers no capability the main surface lacks.
number: F1
binding: must
role: load-bearing
applies_to: [builder]
---

# F1 · An emergency exit, not a second API

**What holds.** A service MAY offer a second surface under its own
prefix for callers that can send nothing but `GET` without headers of
their own. If it does, the surface is explicitly named as a detour, the
main surface remains the first one, and the service's entry page points
to it with one sentence that says what it is for: *If you can only send
`GET`, read on there.* The second surface is a pass-through: each of its
calls invokes the operation the main surface has for the same purpose;
it implements nothing itself and offers no capability the main surface
lacks. The only permitted exception to "invokes" is collecting the parts
of an upload ([F5](./F5-the-upload-in-parts.md)), and that, too, is an
operation of the storage layer, not of the surface. A structural test in
the build enforces this: it fails when an operation is performed on this
surface instead of invoked, and it requires by name that the depositing
call goes through the same operation as on the main surface.

**Why.** The second surface gives up two things the main surface has,
the secret in a header and verbs according to their meaning, and
neither may rub off on the main surface. Under its own prefix every
property given up is in the description of *this* surface, and the
name says in the log what you are looking at. That it implements nothing
itself is not tidiness: two implementations of the same operation drift
apart as soon as someone touches only one, and it goes unnoticed because
each works on its own; the author collected ten such places in one day
on another pass-through of the same service. And offering no new
capability keeps it an emergency exit: as soon as you can do something
on it that you cannot do otherwise, it is a second API with worse
properties.

**Considered and rejected.** *Extend the main surface with `GET`
variants*: then the secret is at every address of the main surface
and `GET`s that take effect mix with the reading ones; the concessions
taint the whole API. *Leave it and mention it in the guide*: excludes a
type of tool that is increasingly the only one an agent has in a
sandbox. *Put a proxy in front that adds the headers*: it must be
installed and supplied with the secret; exactly the setup project that
self-description does away with. *Build the second surface "more
complete" while you are at it*: then it is no longer a pass-through, and
the main surface loses its precedence.

**How you notice it is violated.** A caller chooses the second surface
although it could use the first, because it has something the first does
not; or an operation behaves differently on the two surfaces, and nobody
decided that.
