---
type: Proposal Rule
description: A write that needs a structured body becomes a composition, created, filled field by field with one route per field, shown at any time and deposited by exactly one call that hands it to the main surface's operation; nothing can be removed.
number: F6
binding: must
role: load-bearing
applies_to: [builder]
---

# F6 · The composition: filled field by field, shown at any time, deposited by exactly one call

**What holds.** An operation that takes effect and needs a structured
request body on the main surface arises on the second surface as a
**composition** over several fetches. One call **creates** it and
answers with its identifier: `<base>/get/v1/<token>/compose`. **One
route per field** sets or adds a value: `…/compose/<id>/subject/<base64url>`,
`…/to/<address>`, `…/attach/<file-id>` (the fields of the pattern; a
mail request serves as the example throughout). Fields that hold several
values **append** (in the pattern: one recipient per call), fields with
one value **replace**.
A call without an action **shows** the composition as it stands, so that
the caller can put it before its user. And **exactly one** call
**deposits** it, `…/compose/<id>/deposit/<key>`, and hands the finished
composition to the same operation the main surface uses for this
request. Until then nothing has gone out, and this one address is the
place where the user's consent belongs. There is **no** way to remove
anything from a composition: a caller that errs starts a new one; nothing
abandoned is ever deposited, and an abandoned composition expires in
the staging area like an unfinished upload. A composition that is not
yet a valid request is refused on depositing, and the refusal lists
what is missing. The description MUST say that there is no removal,
instead of letting the caller search for it.

**Why.** The objection to writing `GET`s was weighed, not passed over:
an accidentally triggered fetch, by a chat's link preview, by
prefetching, by a repetition, fills a field of a composition and does
nothing else. What remains is "deposited before the user agreed", and
the design helps against that: the composition is built over many fetches,
and **exactly one** fetch deposits. That one address can be named, it is
the only irreversible one, and the description can highlight it. One
route per field, because then the case "two values for one field in one
call" cannot arise and the path itself says which kind of value it
carries ([F3](./F3-two-encoding-rules.md)); one recipient per call,
because a repeated parameter in the first version silently kept the last
one and swallowed a recipient ([F9](./F9-no-guessing.md)). No removal,
because every undo operation would be a second capability the main
surface does not have ([F1](./F1-an-emergency-exit-not-a-second-api.md)),
and because "start over" costs nothing on a surface on which nothing
abandoned ever takes effect. And the state is shown, because the user's
confirmation must refer to something they see.

**Considered and rejected.** *The whole request in one address
(`…/send/<from>/<to>/<subject>/…`)*: fits one line of text and nothing
else, and makes every fetch one that takes effect. *One field with two
parameters ("text as a value or as a file")*: the first version; with
both given, the service took the file and dropped the text, silently
([F9](./F9-no-guessing.md)). *Being able to remove fields*: one
capability more than the main surface, for a case that "start over"
solves more cheaply. *Delete the composition on depositing*: then a
repetition after a dropped connection says "does not exist", and the
caller does not know whether the request took effect
([F10](./F10-repeatability-without-headers.md)). *The user's consent at
every field*: overwhelms the user and protects nothing; only the one
call takes effect.

**How you notice it is violated.** A caller asks how to get a recipient
out again; or a prefetch has deposited something; or the user's consent
is obtained at a place that has no effect, and not at the one that does.
