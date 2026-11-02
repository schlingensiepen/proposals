---
type: Proposal Scope
description: Security considerations, what the two proofs from outside cost, where the concept applies, and what is not yet settled; several parts are not yet written.
---

# Scope

## Security considerations

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Outside test with a live secret | the test agent from [F2](./rules/F2-acceptance-by-a-reader-without-inside-knowledge.md) really acts (files, writes, sends) and nobody watches it do so | a narrowly scoped token for a target where a wrong move costs nothing; handed over like any other secret; test tasks that put no load on a third-party system |

## What it costs

**Every check is written twice
([F1](./rules/F1-a-check-that-cannot-go-red-is-none.md)).** F1 demands
that every new check run once with its safeguard removed and fail. That is
a second pass per check: remove the safeguard by hand, watch the run,
restore the safeguard, and where the check stayed green, sharpen it and
start again. It costs minutes for every check and an hour for some, and it
costs discipline at the point where discipline is scarcest: after the
check is written and shows green. If you skip the red probe, you save this
time and get checks of which an unknown share tests nothing.

**Every round ends with an outside run
([F2](./rules/F2-acceptance-by-a-reader-without-inside-knowledge.md)).**
F2 demands, at the end of every round, an agent that has nothing but what
any later reader will have and a task. The run itself is cheap; its
preconditions and its evaluation are not. Where the test touches a live
system, it needs a target on which the agent may really act without doing
damage, a narrowly scoped secret for it and its handover; it needs someone
who reads the report, because the tester reports symptoms in the user's
language and the cause lies behind them; and it needs the willingness to
treat a finding as a bug that looks like a misunderstanding from the
inside. The outside test also finds only what the task given touches: a
surface no test task ever enters stays untested, and nobody notices until
a consumer enters it.

## Where it applies

### Preconditions

**A reader without inside knowledge.** The builder must be able to reach a
tester who has nothing but what any later reader will have and a task, as
a rule an agent with no project context preloaded. This carries the answer
because F2 makes the acceptance of the document depend on it, and because
the builder, by their own observation, cannot do it themselves: they read
the document with the very knowledge it is meant to replace. You recognize
the situation when every available tester already knows the source code or
the specification; then what they find is a code review, not an outside
test.

### When this no longer holds

Not yet written.

### What it does not solve

Not yet written.

### Not yet settled

* F2 demands the outside test but does not say what the test task says,
  how many tasks a round has to cover, or when a surface counts as
  accepted. Should the concept supply a model task ("here is a URL and a
  secret; do X and report your path") and a stopping criterion, for
  instance two consecutive runs without a finding? And how do you keep a
  model task from settling into the same paths the builder has checked
  anyway, so that the tester stops being an outsider?
