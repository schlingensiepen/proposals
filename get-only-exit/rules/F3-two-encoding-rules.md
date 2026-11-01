---
type: Proposal Rule
description: Identifiers, addresses, numbers and names travel readable in the path; free text that may contain a slash travels as base64url without padding; a segment that could contain a slash reaches the handler and is refused with the rule.
number: F3
binding: must
role: load-bearing
applies_to: [builder]
---

# F3 · Two encoding rules: identifiers and addresses readable, free text encoded

**What holds.** Identifiers, addresses (such as e-mail addresses),
numbers and names stand readable in the path, with ordinary percent
encoding. **Free text**, anything that may contain a slash (a subject, a
text body, the content of a file part), travels as **base64url** without
padding. A name that travels readable MAY NOT contain a slash, and if it
does, the refusal says so. Every path segment in which a slash could
occur is routed so that the call *reaches* the handler and gets a
refusal with the rule, instead of ending at a non-matching route with a
bare `404`. The description of the surface shows how to encode, with
one line that can be copied.

**Why.** The rule is measured, not chosen: a percent-encoded slash in a
path segment is decoded before routing; the route no longer matches
afterwards, and the caller gets a `404` that tells it nothing. Umlauts,
line breaks, `#`, `?` and `&`, by contrast, survive percent encoding
unchanged. A subject may contain a slash, a file identifier may not; so
the subject needs an encoding that cannot produce a slash, and the
identifier needs none. base64url was already the form of the file parts;
taking it for subject and text as well keeps the number of rules at two.
Two encoding rules instead of one are the price for no value silently
failing on a character.

**Obvious and rejected.** *Percent-encode everything*: fails on the
slash, and with the answer that says least. *base64url for everything*:
makes every address unreadable, including the ones a person should
recognize in a history, and encodes identifiers that never contain a
critical character. *Forbid the slash in free text*: a subject like
"Q3/2026" is not a special case but everyday. *Invent a replacement for
the slash*: a third rule nobody knows.

**How you notice it is violated.** A call with a slash in the value gets
a `404` without explanation; or callers encode identifiers because the
description does not say which values travel readable.
