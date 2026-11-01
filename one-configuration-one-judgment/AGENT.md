---
type: Proposal Agent Guide
name: one-configuration-one-judgment
description: Self-contained guide for an agent that adopts this concept in a service; includes the mandatory step of deciding the two choices with the user.
---

# Agent guide

## Purpose

You are building or revising a service that reads operating parameters
before it starts, and you are adopting this concept. This guide is
complete on its own, except that step 2 needs the two choice files:
they carry the options with their consequences. The rule files are the
reference behind the rest, named in parentheses.

## Procedure

1. **List the parameters.** For each: name, meaning, default or
   required. A parameter without a usable default is required.
2. **Decide the choices with the user.** Do not decide them yourself.
   Group the parameters by the answer that fits them, present each
   group with the options, their consequences and the condition under
   which each fits, and record the user's decision per group; record
   deviations of single parameters separately.
   * Precedence (W1): file precedence, all values from one selected
     file; or per-value precedence, each value from command line,
     environment, user file, system file in that order. If in doubt:
     file precedence for a hand-operated service, per-value for a
     platform-operated one.
   * Empty value (W2): empty means the default applies; or empty means
     set and invalid, the start aborts. Required parameters are always
     the second.
3. **Build one reading logic** that both the start and the check use.
   The check must not have its own parser, schema or defaults (F1).
4. **Build the check output.** For every effective value: name, value,
   source. Sources are the command line, the environment variable by
   name, the file by path, "default", or "default (set empty)" where
   the user chose that empty means default (W2 a). With per-value
   precedence, when a setting is given at more than one rank, name the
   lower rank and its place as well (F2, F5). With file precedence, when
   no file is found, say so and report every value as a default. Never print a secret, not even truncated;
   print where it is referenced from (F3).
5. **Keep the check free of side effects.** No port is bound, no state
   is written (F4).
6. **Document** the parameter list with defaults, the environment names
   if per-value precedence was chosen, and the recorded decisions.

## Checklists

**Judgment (F1)**
* The check and the start call the same function to load and validate.
* A configuration the check rejects is one the start rejects, and vice
  versa; test both directions with one invalid and one valid file.
* The check prints defaults as effective values.

**Source (F2)**
* Every command prints the path of the file it read.
* With per-value precedence, every value prints its source.

**Secrets (F3)**
* No secret value is in the configuration file; only a reference.
* The check output contains no secret, run it with a known secret and
  grep for it.

**No binding (F4)**
* The check runs successfully twice in a row while the service is
  running.

**Duplicate setting (F5, per-value precedence only)**
* Give one setting in the file and in the environment; the check
  reports the environment as winning and names the file as also set.

**Choices (W1, W2)**
* Every parameter has a recorded decision for W1 and W2, grouped.
* Every required parameter aborts the start when set empty, with a
  message that names the parameter and the next step.

## Self-check

Before you report completion: can an operator, from the check output
alone, say for every parameter which value is in effect and where it
came from? Can the user find the record of the choices? If either is no,
you are not done.
