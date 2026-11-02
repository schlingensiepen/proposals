---
type: Proposal Scope
description: Security considerations, what the concept costs each role, where it applies, when this no longer holds, what it does not solve and what is not yet proven.
---

# Scope

## Security considerations

The concept carries no security function, but it prevents two classes
of mistakes that cost security: the silent fallback (a service runs with
a default nobody wanted, such as the data directory inside the container
instead of on the volume) and the second truth (a validation tool that is
trusted until, in an incident, it reads differently from the service).
[F3](./rules/F3-secrets-are-referenced-not-contained.md) makes sure the
duty to report does not become a query surface for secrets.

The price of per-value precedence
([W1 b](./choices/W1-precedence-file-or-value.md)) is a larger attack
surface through the environment: anyone who can set the process's
environment variables can override any value. With file precedence this
is limited to the one variable that selects the file. If you choose
W1 b, you accept this and limit it through the permissions on the
process definition.

## What it costs

The builder: one shared reading logic for start and check (usually the
cheapest build anyway), an output with a source per value, and with
W1 b a documented mapping from each parameter to its environment name. The
operator: the decision per group for W1 and W2, once, at adoption. The
adopter: nothing; they get the report they would otherwise be searching
for.

What it does not cost: a file. The concept prescribes no file; it
prescribes that every value has a known source.

## Where it applies

**Conditions.** Any service that reads operating parameters before the
start, regardless of build form, language and process form. It
presupposes an invocation that can check without starting
([F4](./rules/F4-check-without-binding.md)); in a service with several
modes that is the check command.

**When this no longer holds.** Where the configuration is changed at runtime
(switching over a network surface, hot reload) there is no longer one
judgment at start but one per change; the concept then applies to each
of those judgments, but the duty to report becomes a duty to log. Where
a platform owns the configuration entirely (an orchestrator that injects
values and knows no file), W1 b is the only possible choice.

**What it does not solve.** Where secrets come from
(`where-a-secret-comes-from`). What state is and where it lives
(`service-as-single-artifact`, rules on state). How configuration is
changed at runtime. And, by design, the mechanics: file format, file
paths, the layout of the check output, exit codes; those are the
builder's.

**Not yet proven.** Whether [W1](./choices/W1-precedence-file-or-value.md)
options (a) and (b) should be mixed within one service (one group from a
file, one per value) or whether that costs too much explanation. It is
permitted; experience is missing.
