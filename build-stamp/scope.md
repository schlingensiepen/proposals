---
type: Proposal Scope
description: Security considerations, what the build stamp costs, where it applies, and what is not yet settled; several parts are not yet written.
---

# Scope

## Security considerations

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Statement about the wrong revision | an image runs at an older revision than the one tested; the acceptance applies to something else | Build stamp in the artifact, report in the version command, smoke test before publishing (F1) |

The rule keeps the stamp out of the unauthenticated readiness point: the revision is an operational internal, and wherever a network surface reports it, that surface is authenticated (F1).

## What it costs

**The build stamp has to be carried through every form of build (F1).** The build pipeline has to pass the revision in, the image has to carry it, the command has to report it, the smoke test has to check it: four places that must fit together. And the smoke test costs seconds to minutes on every build, plus a running container environment in the build. A locally built artifact says "unknown", which is right on a development machine and unsatisfying for whoever tests there.

## Where it applies

### Preconditions

Not yet written.

### When the answer flips

Not yet written.

### What it does not solve

Not yet written.

### Not yet settled

* The rule demands the stamp in the version command, not in the readiness point. For an operator who reaches the running instance only over the network, this means the question "which revision is running?" cannot be answered without a one-off invocation, unless an authenticated network surface reports the revision. Does the rule need a second way for that, for instance the stamp in a response that only a valid access receives anyway, or is this the boundary at which operation without a one-off invocation flips in any case?
