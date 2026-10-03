---
type: Proposal Agent Guide
description: Self-contained guide for an agent that adopts this concept in a service, a script or an agent; includes the mandatory step of deciding the three choices with the user.
---

# Agent guide

## Purpose

You are building or revising a service, a script or an agent that needs
a secret before its first step, and you are adopting this concept. This
guide is complete on its own, except that step 2 needs the three choice
files: they carry the options with their consequences. The rule files
are the reference behind the rest, named in parentheses.

## Procedure

1. **List the start secrets and group them.** Three groups usually
   suffice: inherited keys to target systems (the service cannot
   generate them), own root secrets (administration, setup), accesses to
   the function (the service issues them in operation; they do not
   belong on the list). Anything that only arises in operation the
   service keeps in its own state.
2. **Decide the choices with the user.** Do not decide them yourself.
   Per group, present the options, their consequences and the condition
   under which each fits, and record the decision. "The user" here is
   the person you are working for, usually the operator or the builder.
   * No secret set (W1): refuse to start; or generate and show exactly
     once. Generate only for secrets whose only possible owner is the
     service; inherited keys are always "refuse to start".
   * Set but empty or unusable (W2): abort; or warn and continue.
     Everything that opens an access or reaches a target system is
     always abort.
   * Further sources (W3): a rank of its own; or a clause. A rank of its
     own only where the service addresses the manager itself.
3. **Per secret, fix the environment name** (pattern
   `<SERVICE>_<PURPOSE>`), document it, and build the file path as a pure
   pass-through: path from `<SERVICE>_<PURPOSE>_FILE` or a documented
   default in the state directory, owner-only read or the start refuses,
   content flows straight into the call (F2, F3).
4. **Build the start report.** Per secret: name, rank in effect; if a
   lower one is also set, its path and the way to clear it (F4). For a
   file the service wrote itself, a way to clear it without a shell: an
   agreed command word in the variable (say `new`) and a restart; the
   word is exempt from the usability check and regenerates at every
   start while it is present, which the report says (F5). For a file the
   operator placed, the report names the path; the service never deletes
   on its own.
5. **Where W1 (b) is chosen:** generate (high entropy, recognisable
   prefix), store at the rank 2 path, exactly one log line
   (`<NAME> generated: <value>. Shown once; it will not be shown
   again.`); never at an ordinary restart, never for a secret that was
   set. Unusable means shorter than 32 characters unless you and the
   user agree otherwise (W2). Smoke test in the build: appears exactly once, is
   accepted, still applies after a restart, does not appear again.
6. **Search.** Run with a known secret; search output, logs, error
   messages, transcripts and addresses for it. The only permitted hit is
   the one line from step 5 (F6).
7. **For agents:** secret from the environment, else from a file as a
   pass-through, only last from the context window; then persist
   immediately, never repeat, never in output (F1 to F3, F6).

## Checklists

**Ranking (F1, F2)**
* Per secret, the canonical environment name is documented.
* The order environment, file, once-only channel is one function in the
  code, not scattered.

**Pass-through (F3)**
* The file is expected with tight permissions; its content is never
  logged, never built into an error message.

**Never hides (F4)**
* Set both the variable and the file; the start report names the
  variable as in effect, the file as also set, its path and the way
  to clear it.

**Two moves (F5)**
* W1 (b): set the variable to the command word, restart; the file is
  replaced and the new secret appears once in the log; the report says
  to remove the command word.
* W1 (a): the start report names the path of the lower-rank file so the
  operator can clear it on their platform.

**Never in any output (F6)**
* Searching all output for the known secret returns at most the one
  line from W1 (b).

**Choices (W1 to W3)**
* Per group, a recorded decision.
* An inherited key without a value aborts the start and names the
  variable.
* A root secret shorter than the agreed threshold aborts the start and
  is not silently replaced; the command word does not.

## Self-check

Before you report completion: can an operator say, from the start report
alone, how many valid secrets their instance has right now? Can they
replace one without the old one still applying? Can the user find the
record of the choices? If any of these is no, you are not done.
