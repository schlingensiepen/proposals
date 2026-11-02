---
type: Proposal Evidence
description: Where the concept has held; the reference implementation on the author's mail-depositing service, what it proves and what it does not, the incidents it came from, and the honest state.
sources:
  - id: reference-implementation
    url: https://github.com/schlingensiepen/imap-service
    title: The author's mail-depositing service (not publicly accessible)
---

# Evidence

This file separates what is built and in use, what remained open about
it, and what serves as illustration. The surface is young: it was built
in September 2026, rebuilt from the query form to the path form a few
days later, and has since been part of a service running under a public
name. There is no operating experience with outside consumers; what
there is, are two reviews from outside and a handful of findings from which
half of the rules stem. Nothing in this file claims as built what the
writer has not seen in the source code.

## Origin

The concept comes from building the surface, not from reflection. The
first version took its values as query parameters: the secret as
`?t=…`, every value as `&v=…`. The author discarded that four days
later, for two reasons, both measured and not assumed. First, many
sandboxes in which agents run forbid or strip the query part of a `GET`
address; a surface that exists for weak tools must not fail on exactly
the weakness it exists for. Second, a measurement decided the design of the
path: a percent-encoded slash in a path segment is decoded before
routing; the route no longer matches afterwards, and the caller gets a
`404` that tells it nothing about the cause; umlauts, line breaks, `#`,
`?` and `&`, by contrast, survive percent encoding unchanged. A subject
may contain a slash, a file identifier may not; so free text travels
encoded and everything else readable
([F3](./rules/F3-two-encoding-rules.md)). As a side gain, one field with
two parameters became two routes: the case "both given", which a
reviewing agent had reported as silent guessing, can no longer arise
([F9](./rules/F9-no-guessing.md)). And the thing that arises over many
fetches was first called "draft", until a reader asked whether the
completing call should not be named as with the file upload. It has
since been called **composition**, and the reason is recorded as a rule
([F7](./rules/F7-the-word-composition-not-draft.md)).

## The reference implementation

**What it is.** The author's mail-depositing service (it takes files and
mail requests over HTTP, builds messages from them and deposits them
over IMAP into a mailbox folder) has carried, since September 2026, a
second surface under its own prefix on which everything goes through
`GET`. Built and seen in the source code: the own prefix with the token
as the first path segment and without any query parameter; a prose
description under the prefix retrievable without a secret, rendered
with the base URL of the request, `text/plain` by default and
`Vary: Accept`; a sentence on the service's entry page pointing to it
("If you can only send `GET`: the same service through a narrower door,
described in prose: no headers of your own, no request body, the token
in the address. Take it when your tool leaves you no choice."); three
reading calls that invoke the same functions as the main surface and
answer as text by default and as structure on `application/json`, with
refusals in the one error format in both cases; the upload in parts with
opening, numbered appending and finishing, the parts in a staging area
swept after one hour, assembly in numeric order, the handover of the
finished bytes to the same upload as on the main surface, the refusal of
a differing part and the named gap; the composition with one route per
field, appending recipient fields, replacing single fields, showing the
state, the depositing call with a path-safe key that goes through the
same operation as the main surface, the receipt kept on the composition,
the refusal of an unfinished composition listing what is missing, and a
refusal of unknown actions that lists the existing ones; free text as
base64url, names without a slash, and for every segment that could
contain a slash a refusal with the rule instead of a bare `404`; a
structural test that forbids the surface to perform an operation itself
or touch the file system, and that requires by name that the depositing
call goes through the one operation; and the coupling to the switch of
the self-description, with which the surface disappears as a whole. The
*unauthorized* refusal names the path form of the token and not the
header.

**What it proves.** Touched and in operation are F1 (pass-through with
structural test, pointer on the entry page), F2 (path form, token
first), F3 (two encoding rules, explained slash), F4 (mirror with
`Accept`), F5 (upload in parts, staging area, handover to the one
upload), F6 (composition with exactly one depositing call, no removal),
F7 (the renaming is done in specification, routes and description; in
some test names the old word lives on), F9 (one route per field instead of a
precedence rule, one recipient per call), F10 in key and receipt (key in
the path, receipt on the composition; the refusal of a key already used
for a different composition is **not built**: the reference answers such
a call with the old identifier), F11 in duties (1) and (2) (the price is in the
first paragraphs of the description, and the main surface accepts no
token from an address) and F12 in description, rendering, delivery form
and coupling to discovery. F8 is **partly** met: the texts were written
anew for this surface, and a test demands that the *unauthorized*
refusal does not name the header and that the two *not found* refusals
contain no `POST`; but three refusals still name an address form from
before the rebuild, without the token segment; the test asks for the
name of the call, not the form of the address, and therefore did not
notice. **Not built** are duties (4) and (5) of F11: there is no
separate token per surface and no marking of a token for this surface
(the same token holds on both), the surface's answers carry no header
that forbids caches to keep them, and shortening the secret segment in
own logs does not exist. F11 duty (3) follows from the scope of the
service's tokens (one token is one mailbox folder), not from a rule of
the surface. And the checks the service runs over its usage description
do **not** run over the prose of this surface; its own tests demand the
chain of identifiers and some mandatory sentences, no more.

## Incidents, stripped down

* *(F2, F3)* The surface first took its values as query parameters. The
  author discarded that because many sandboxes strip the query part of a
  `GET` address, and a measurement decided the design of the path: a
  percent-encoded slash in a path segment is decoded before routing, the
  route no longer matches, the caller gets a bare `404`; umlauts, line
  breaks, `#`, `?` and `&` survive the encoding. Since then free text
  travels as base64url, everything else readable. Price: a rebuild of
  the whole surface four days after the first build.
* *(F7)* The thing was called "draft"; a reader asked whether the
  depositing call meant "save as draft" and whether it should not be
  named as with the upload. Price: a renaming across specification,
  code, tests and description.
* *(F8)* The surface took over the main surface's error texts word for
  word; they recommended a header, another call and a repeated header.
  A reviewing agent called it the most serious finding of its run.
* *(F9)* A field with two parameters, with both given, took the file and
  dropped the text; a repeated recipient parameter kept the last one.
  The same reviewing agent reported both; both have been inexpressible
  since the rebuild to one route per field.
* *(F4)* The description was prose, the answers were structure, and it
  said so nowhere; a reader asked after the first data call whether text
  should not have come back there, and a second time after only the
  description had been amended. Since then the answer follows `Accept`,
  text by default.
* *(F12)* Reading the finished text, the author found that the
  identifier of the call that takes effect was named twice for
  substitution and never handed out; the first test for it stayed green
  because the word stood in the neighboring sentence, and was sharpened
  until it turned red with the sentence removed. The same reviewing
  agent had to guess four things (the encoding, the form of the key, the
  absence of any undo, that a finished file stays where it is), guessed
  all four correctly and said so; all four are now in the text, each
  with a test.
* *(F1)* A reading call was written out instead of taken over from the
  main surface, and a malformed identifier became a server error there,
  where the main surface answers *not found*: the finding that contains
  the argument for the pass-through in one line.

## The honest state

Built, rolled out, in operation since September 2026: first in the
query form, since the rebuild a few days later in the path form. Checked
from outside **once**, and that in the first form: an agent got nothing
but the address of the surface and a token, was allowed to send nothing
but `GET`, and was to put a message with an attachment into a real
mailbox. It finished at the first attempt (the message lay in the folder
after 340 milliseconds, umlauts intact through address, structure and
IMAP, the attachment's checksum byte for byte equal to that of its local
file) and afterwards reported six findings, all confirmed and fixed. The
**path form has had no external check**: the author walked through it
in person from start to finish after the rebuild, with a subject that
contains a slash; that is a run by the builder, not an acceptance under
[F12](./rules/F12-the-description-is-the-only-document.md). Not met and
named as such: the three refusals with the old address form (F8), the
separate token per surface and the marking on issue (F11 duty 4), the
header against caches and shortening in own logs (F11 duty 5), the
checks against inside knowledge and hindsight over the prose of this
surface (F12), the test names with the old word (F7), the refusal of a
used key with a different composition (F10). Not proven,
because nobody but the author's agents has used it: that the surface
reaches the tools it was built for. The sandboxes that strip the query
part are the reason for the rebuild, but a run from inside such a
sandbox is not recorded.

The repository of the reference implementation is not publicly
accessible.
