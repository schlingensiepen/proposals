---
type: Proposal Rule
description: A file arrives in numbered parts that are opened, appended and finished; only finishing hands the assembled bytes to the main surface's upload, so the store never sees half a part; the description states the scale.
number: F5
binding: must
role: load-bearing
applies_to: [builder]
---

# F5 · The upload in parts: open, append numbered, finish; the store never sees half a part

**What holds.** Because an address does not carry a file in one piece, a
file arrives in parts. One call **opens** an upload and answers with its
identifier: `<base>/get/v1/<token>/newfile/<name>`. One call per part
**appends** a part, with the part's number before the content:
`…/upload/<id>/part/<n>/<base64url>`. One call **finishes**,
`…/upload/<id>/finish`, and hands the assembled bytes to the same
operation the main surface uses for an upload; only then does an entry
appear in the store, and the answer is the same as there (identifier,
size, checksum). The number is the part's **number**, not its order of
arrival; assembly is in numeric order. A part sent a second time with
the same number and the same content has no consequence; with different
content it is refused. If a number is missing on finishing, the call is
refused, and the message says which. Until finishing, the parts lie in
the staging area and do not reach the store; an abandoned upload is
swept away by itself after a fixed period. The description MUST state
the scale, roughly three kilobytes of payload per call, so about a
hundred calls for a document of three hundred kilobytes, and say that
the main surface's upper limit cannot be reached this way.

**Why.** The store must never see half a part, because everything there
is taken to be complete, and because size, checksum and metadata should
arise in exactly one place, namely in the operation the main surface
uses too. The staging area is the right place for this: what lies there
is incomplete by definition, and a sweeper that empties it anyway takes
an abandoned upload with it, without anyone writing a second one. The
number rather than the order, because with a hundred calls no call may
get lost or overtake another without it being noticed; otherwise the
correctness of the file would hang on an assumption you must not make
with a hundred calls; and because a lexicographic order puts part 10
before part 2 and assembles the file wrongly without anything failing.
The repeated part has no consequence, because a caller that has not
seen an answer must be able to repeat the call; the differing one is
refused, because one of the two is an error, and guessing would produce
a file nobody sent.

**Considered and rejected.** *The whole file in one address*: addresses
carry a few kilobytes, not megabytes; every intermediary has a
limit. *Write parts straight into the store and concatenate at the
end*: then something incomplete lies there for a while, and the store
has two kinds of entries. *The order of arrival as the order*: breaks at
the first overtaking call, silently. *Assembly in the surface itself*:
then the surface writes into state directories; that belongs to the
storage layer, and a structural test keeps it there
([F1](./F1-an-emergency-exit-not-a-second-api.md)). *A separate sweep
for uploads*: the staging area already has one.

**How you notice it is violated.** The store holds a file whose
checksum does not match its content; or a caller gets a refusal on
finishing that does not say which part is missing; or an abandoned
upload is still there weeks later.
