---
type: Proposal Evidence
description: Where the concept has held; its origin in an implementation by the author, three green checks that tested nothing, nine rounds of self-review against the first outside run, and the series testing every bundle this way.
---

# Evidence

## Origin

Both rules come from an implementation by the author, August to September
2026: a small service, built, deployed under a public name and used over
several rounds by agents that had nothing but the URL and a token. They
first appeared in a revision proposal for `self-describing-services` of
September 2026 and were made a concept of their own because they hold for
every check and every document meant to work without inside knowledge, not
only for a service's description of itself. The repository of the
implementation is not publicly accessible.

The rules were *gained* from that service, not confirmed on it: each
answers an incident that happened during the implementation.

## Three green checks that tested nothing

Three assertions stayed green when what they tested was removed: one for
"something was logged", one for a name that was in the following
sentence, one for a rejection that came one stage later anyway. This is
the case behind
[F1](./rules/F1-a-check-that-cannot-go-red-is-none.md).

## Nine rounds of self-review and the first outside run

Nine review rounds by the developer over their own work found a great
deal; in eight of them the worst finding was inside the fix for the
previous error. This count is the author's observation during the work,
not a kept record, and is given here as a report of experience. The
substantial findings came from a real mailbox, from a reader of the
finished text, and from agents with nothing but the URL and a token; the
first such run against a fresh surface produced six findings, among them a
server error on malformed input. This is the case behind
[F2](./rules/F2-acceptance-by-a-reader-without-inside-knowledge.md).

## The series tests every bundle this way

The series this concept belongs to applies F2 to its own documents.
Before a bundle is published, an agent without context gets nothing but
the bundle directory and a task in the user's language (a small service to
be built according to the concept), adopts the concept, and reports its
path, the questions it would put to the user, and what it was missing; the
findings are fixed before publication. Each of the first three such runs
found what the authors' own review had missed, for example a command word
shorter than the minimum length it had to pass itself, and rules that
still contradicted a decision taken after the first version. The tester
had neither a URL nor a secret, only the directory itself: what any later
reader has.

## Not yet proven

Whether the rules are sufficient will only show in operation with outside
consumers, which the implementation they come from does not have. A red
probe for the series' own mechanical checks is not documented.
