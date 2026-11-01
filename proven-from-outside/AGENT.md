---
type: Proposal Agent Guide
name: proven-from-outside
description: Self-contained guide for an agent that writes checks or a document meant to work without inside knowledge, or reviews them; two steps, the rules, an acceptance checklist and common mistakes.
---

# Agent guide

## Purpose

You are writing checks, or a document meant to work without inside
knowledge (the description of a service, an instruction, a concept), or
you are reviewing whether existing checks test anything and whether such a
document has been accepted. This guide is complete on its own. Rules are
named in parentheses as provenance.

## Procedure

**1 · See every check go red once.** Before you trust any check, whether a
concept you are following demands it or you added it, run it once with its
safeguard removed and watch it fail. If it stays green, sharpen it until it
tests the one statement only the guarded side can satisfy, not one that
happens to be true without the rule, and run it again. *Result:* for every
check, one observed red run. (F1)

**2 · Close every round with an outside run.** At the end of every round in
which the document or the interface changed, hand an agent with no project
context the URL, one narrowly scoped secret for a target where a wrong move
costs nothing, and one task phrased in the user's language; give it no
source code, no specification and no conversation with you. It completes
the task and reports its path. Read the report as a bug list, fix, and run
it again after the next change. *Result:* an accepted version and a bug
list. (F2)

**Decide the choices.** This concept has no choices.

## Rules

MUST: run every check once with its safeguard removed while you write it,
and see it fail; sharpen any check that stays green · accept a document
only after an agent with nothing but the URL, a secret and a task in the
user's language has completed the task and reported its path · do this at
the end of every round in which the document or the interface changed ·
treat the findings as bugs, not as opinions · give the tester a narrowly
scoped secret for a target where a wrong move costs nothing. (F1, F2)

NEVER: trust a check you have not seen fail; call a document accepted
before someone without inside knowledge has used it. (F1, F2)

## Acceptance checklist

- [ ] Has every check been seen to fail once with its safeguard removed? (F1)
- [ ] Has an agent with nothing but the URL, a secret and a task in the
      user's language completed that task since the last change to the
      document or the interface, and were its findings fixed? (F2)
- [ ] Was the secret for that run narrowly scoped, and its target one where
      a wrong move costs nothing? (F2)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| A test stays green when the guarded code is deleted | Run every new check once with the safeguard removed; sharpen it until it goes red (F1) |
| Self-review round after round, the worst bug sitting inside the previous fix | End every round with an agent that has only the URL, a secret and a task (F2) |

## Self-check

Before you report completion: can you name, for every check, the run in
which you saw it fail? Can you say when someone without inside knowledge
last completed a task with the document alone, and what became of their
findings? If not, you are not done.
