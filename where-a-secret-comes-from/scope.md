---
type: Proposal Scope
description: Security considerations, what the concept costs each role, where it applies, where the answer flips, what it does not solve and what is not yet proven.
---

# Scope

## Security considerations

The concept closes two gaps that have both occurred: the latent access
([F4](./rules/F4-the-higher-rank-wins-and-never-hides.md),
[F5](./rules/F5-replacing-is-two-moves.md)) and the secret in the log
([F6](./rules/F6-never-in-any-output.md)). With
[W1 (b)](./choices/W1-no-secret-set.md) it opens a named exception, and
it names its price: the holder of the generated secret must be able to
finish setting up the system with it, so it is a root; what a root owes
is for whoever introduces it to state. The environment at rank 1 is at the
same time the largest surface: whoever can set the process environment
sets the secret. That is so for every service and is merely spelled out
here.

Against an attacker with the rights of the service none of the sources
protects; the environment is readable to them, the file hangs on the
same rights. The concept protects against accidents, copies and
forgetting, not against a takeover of the process.

## What it costs

The builder: per secret a name, a file path, a start report with a rank
check, a way to clear, and with W1 (b) the generation plus smoke
test. The operator: two moves instead of one when changing, and the
decision per group at adoption. The agent: the discipline to persist a
secret from the context window immediately and never to repeat it.

## Where it applies

**Conditions.** Any service, script or agent that needs a secret before
its first step, regardless of build form and platform. Presupposes a
start at which a report can be made (service), or a moment of obtaining
(agent).

**When the answer flips.** Where a platform owns the process environment
entirely and knows no file, rank 1 stands alone; F4 and F5 then become a
platform question. Where no log leads back to the operator (no return
channel), W1 (b) cannot be chosen.

**What it does not solve.** What a secret may do
(`capability-tokens-for-agents`). How a secret is issued
(`invitation-instead-of-login`). What a root owes (whoever introduces
it, such as an administration surface). Secrets that arise in operation
(the service's own state).

**Not yet proven.** Whether the agent form of the once-only channel
(the context window) works in practice without repetition when the
agent loses its session before it has persisted the secret.
