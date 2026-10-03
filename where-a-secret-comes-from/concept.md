---
type: Proposal Concept
description: The problem of a secret that can live in several places, the concept of one ranking that never hides a lower rank, and the six-step procedure for adopting it.
---

# The concept

## Problem

Every service needs at least one secret at start: the password to the
target system, the key to the store, the administration secret the
operator uses to get in. Every agent that calls a service needs the
access to it. And for both there is more than one place the secret could
be: an environment variable, a file, the platform's secret manager, the
chat in which somebody just mentioned it, a log in which the service
printed it at first start.

Four places in this series stated the same ranking in their own words,
from four angles: the service that reads; the agent that obtains; the
usage description that prescribes; the administration surface that
generates. Each had its part, none the whole, and the gap between them
was exactly the case that goes wrong most often: how does the **first**
secret get in when nobody with a shell is standing at the target? And
what happens when somebody replaces a secret and the old one is left
lying?

The second case has happened. When an administration secret was
changed, the environment variable was changed. The secret generated and
stored at the very first start stayed underneath and would have applied
again as soon as the variable was missing once: on a migration, on
recreating from a template, on a typo. Nobody knew its value, because
the service had shown it only once. A valid secret that nobody owns any
more: a latent access. The ranking had made replacing easy and deleting
invisible.

The problem falls on three roles. **The builder** has to decide where the
service takes a secret from, in which order, and what it does when there
is none. **The operator** has to set a secret, replace it, and be sure
the old one no longer applies. **The consumer**, often an agent, has to
obtain the secret without it ending up in a transcript.

The usual answer is one per service: a section in the documentation,
"set `X_TOKEN`". That answers the first question and none of the others.
The structural answer is a ranking that is the same for every start
secret and every role, and that comes with its price attached: whoever
introduces a ranking owes the report of lower ranks that are also set,
and the way to clear them.

## The concept

The ranking has three ranks, and it is the same for the service and for
the agent. **First the environment**, under a name the service itself
proposes: that is the way a platform with its own secret management
hands the secret in without it ever touching a disk or a log. **Then a
file that only passes through**: tight permissions, content flows
straight into the call, appears nowhere. **Last a channel used exactly
once**: for the service that is its log, into which it writes a secret
it generated itself exactly once; for the agent it is the context
window, from which it takes a secret once, persists it immediately and
never repeats it.

The higher rank wins, and that is convenient. What carries the concept
is the sentence behind it: **it never hides.** If a higher rank is in
effect and a lower one is also set, the service reports it at start,
names the place and says how to clear it. Replacing is therefore two
moves, set at the top and clear at the bottom, and the way for it
exists without a shell as well.

Three questions the concept deliberately does not decide; it offers them
as choices per secret: what happens when **no secret is set** (refuse to
start, or generate and show exactly once); what happens when one is
**set but empty or unusable** (abort, or warn and continue); and whether
**further sources** such as a secret manager are a rank of their own or
a feeder to the first two. The answers depend on which secret it is: an
inherited key to a target system cannot be generated; an administration
secret whose only possible owner is the service can.

And one exception the concept names instead of tolerating: a secret
never appears in output, logs, error messages, transcripts or addresses,
except in the once-only channel where that is chosen, and there exactly
one value, exactly once, only at creation. A rule without an exception
produces either a gap or a workaround.

## The procedure

1. List the start secrets and group them: inherited keys to target
   systems, own root secrets, accesses to the function; anything that
   only arises in operation does not belong on the list (it is kept in
   the service's state).
2. Per group, decide the three choices with the user and record them:
   no secret set (W1), unusable (W2), further sources (W3).
3. Per secret, fix and document the environment name (F2); build the
   file path as a pass-through (F3).
4. Build the start report: which rank is in effect, which lower ranks
   are also set, where they are, how to clear them (F4); the way to
   clear without a shell (F5).
5. Where W1 (b) is chosen: generation, storage at rank 2, exactly one
   log line; verify in the build that it appears exactly once and not
   again after a restart (smoke test).
6. Run with a known secret and search all output, logs and error
   messages for it (F6).
