---
type: Proposal Rule
description: Every job lies completely in exactly one file holding everything needed for execution in already validated form; the file name carries a cryptographically random identifier and a coarse ordering prefix.
number: F1
binding: must
role: load-bearing
applies_to: [builder]
---

# F1 · One job, one file

**Applies to:** builder.

**What holds.** Every job lies completely in exactly one file; it contains everything needed for execution, in already validated form. The intake validates and translates the request once and writes the result; the worker reads the file and does not have to validate again what has already been validated. The file name carries a cryptographically random identifier and a coarse ordering mark (a timestamp in front, say, for approximate order).

**Why.** A job that lies in several files has an intermediate state in which one file is already there and the other is not, and with that the whole question this pattern solves starts over again. And because the file contains what has already been validated, the worker can never fail at a check the intake has already passed: whoever was accepted no longer gets "invalid", only "could not be delivered".

**Obvious and rejected.** *Store the raw request and validate only in the worker*: shifts errors into a context without a caller, to whom they can no longer be reported. *Several files per job (header, payload, attachments)*: every additional file is one more race; attachments belong in a separate, durable pool that the job refers to (F8). *Sequential numbers as identifiers*: they reveal throughput and allow guessing foreign identifiers; a random identifier does neither.

**How you notice it is violated.** A job fails in the worker at something the intake could have seen, or there are jobs that are "half there".
