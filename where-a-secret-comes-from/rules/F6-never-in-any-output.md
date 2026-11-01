---
type: Proposal Rule
description: A start secret never appears in output, logs, error messages, transcripts or addresses; the only exception is the once-only channel under W1 (b): one value, once, at creation. Accesses issued in operation are governed by the concept that issues them.
number: F6
binding: must
role: load-bearing
applies_to: [builder, operator, consumer]
---

# F6 · Never in any output

**What holds.** A secret never appears in output, logs, error messages,
transcripts or addresses (a URL path or query is an address),
not truncated either, not "for debugging"
either. The only exception is the once-only channel where
[W1 (b)](../choices/W1-no-secret-set.md) is chosen: exactly one value,
exactly once, only at creation, with the sentence that there will be no
second time. Never for a secret somebody set; never at restart. Accesses
that the service issues to callers in operation are not start secrets
and not governed here; that the issuing command shows such an access
exactly once is the rule of the concept that issues it.

**Why.** Logs, chat histories and error messages are searchable,
copyable stores; what is in them once is soon everywhere. The exception
must be named, cut narrowly and justified rather than silently
tolerated: a rule without an exception produces either a gap (no way in
for the operator without a shell) or a workaround (the secret in the
image, in the deployment file, in a ticket).

**Obvious and rejected.** *"Secret into the chat" as the normal way*:
the transcript store becomes a key ring. *Logging it at every start
"so one can find it again"*: then the log is a key ring and the
exception is none any more. *Showing it truncated*: the truncation is
switched off at the first incident.

**How you notice it is violated.** Searching logs and transcripts for a
known secret returns more than the one line from W1 (b); or a secret
the operator set appears there at all.
