---
type: Proposal Rule
description: The second surface requires no query parameter; the secret is the first path segment after the prefix on every call, then the thing, the action and the value; an identifier is an address, not a permission.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · Everything in the path, nothing in the query; the secret first

**What holds.** The second surface requires no query parameter, for
anything. The secret is the **first path segment** after the prefix and
MUST be present on every call, including calls that name an identifier:
an identifier is an address, not a permission. After it follows what the
call concerns, then the action, then the value:
`<base>/get/v1/<token>/me`,
`<base>/get/v1/<token>/compose/<id>/to/<address>`. If the secret is
missing or wrong, the surface answers *unauthorized* like the main
surface, but its message names the **path form**, not the header
([F8](./F8-refusals-that-work-on-this-surface.md)).

**Why.** Many sandboxes in which agents run forbid or strip the query
part of a `GET` address. A surface that exists for weak tools must not
fail on exactly the weakness it exists for; the author therefore rebuilt
it from the query form to the path form four days after the first
build. The secret comes first because it is needed on every call, and the
path then reads from left to right like the operation itself: who, on
what, what, with what. And that an identifier does not suffice keeps the
scope of the tokens: a composition of another token is not reachable under
its identifier but answers like one that does not exist.

**Considered and rejected.** *The secret as a query parameter
(`?token=`)*: the first version; it failed on sandboxes that strip the
query part, and as a form it is anyway the one that every place that
stores addresses stores. *Register the secret once and then use a
session identifier*: needs a cookie or a header, exactly what the caller
does not have. *Let identifiers suffice as permission ("anyone who knows
the composition's identifier may fill it")*: turns an address into a
second-class secret that nobody treats as one. *The secret in the
fragment of the address (`#…`)*: a tool does not send the fragment to
the server; the service would never see it.

**How you notice it is violated.** Calls from a particular environment
arrive without values, because the query part was cut off on the way;
or a call with someone else's identifier and one's own secret opens something
that does not belong to the caller.
