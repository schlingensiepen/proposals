---
type: Proposal Concept
description: The problem of configuration read from several places, the concept of a single judgment made by the starting code, and the five-step procedure for adopting it.
---

# The concept

## Problem

A service that is meant to run for more than a few minutes has operating
parameters: the port it listens on, the directory for its data, the
address of the target system, the log level. These values can live in
several places, as an option on the command line, as an environment
variable, in a file near the user, in a file in the system, and each
place has its purpose. The file can be versioned, the environment is the
platform's way in, the command line is the way when trying things out.

The trouble starts as soon as there is more than one place. Two
configuration files on one machine, and you reliably edit the wrong one.
An environment variable nobody remembers silently overrides the value
from the file. A validation tool says "fine" and the service still does
not start, because the tool reads differently from the service. And the
quietest case of all: a container platform sets a variable to **empty**
because the value it was supposed to substitute does not exist on the
host; the service treats empty as unset, takes the default, runs, and
three months later someone asks why the data is not on the volume they
mounted.

Three roles carry the problem. **The builder** writes the reading logic
and has to decide what holds when two places disagree. **The operator**
sets values and needs to know whether they arrived, before the restart,
not after. **The adopter** finds a running service and must be able to
say which value is in effect right now and where it came from.

The usual way out (a schema, a validation tool, a documentation page of
environment variables) softens the problem and creates a second truth
about the configuration that agrees with the first only as long as
someone maintains both. The structural answer goes one level deeper:
there may be only **one** judgment, and it must be made by the code that
starts on the configuration.

This proposal is an importable base concept. It says nothing about what
a service does or how it is built, only about how it judges its
configuration and reports on it. Other proposals in the series
presuppose it (see [sources](./sources.md)).

## The concept

Everything begins with the check: **the verdict on the configuration is
whatever the start does.** Whatever checks the configuration is the same
code that starts on it. What the check accepts starts; what it rejects
does not. And the check shows the effective values the way the start
will use them, defaults included, because a value nobody set is in effect
all the same.

From this follows the duty to report: **the check names the source.**
Which file was read appears in every output; where values can come
individually from several places, each value says where it came from.
Not because that is elegant, but because hunting for the place a value
really comes from is the most common time sink in troubleshooting.

Two things the concept deliberately does **not** settle; it offers them as
choices: whether precedence selects the **file** (all values from one
file) or applies **per value** (each value individually from command
line, environment, file), and what an **empty value** means (the default
applies, or set and invalid). Both questions have different right
answers for different settings; the port may treat empty as default, the
data directory may not. So anyone who adopts the concept decides them per
setting, in practice per group of settings, and records the decision
([choices](./choices/), [AGENT.md](./AGENT.md)).

The concept draws two boundaries. Secrets are not in the configuration;
they are referenced from it, and where they come from is the business of
the sister concept `where-a-secret-comes-from`. And the check binds
nothing: no port, no state; it can be repeated at any time while the
service runs.

## The procedure

1. Write down the operating parameters: name, meaning, default or
   required.
2. For each parameter decide the two choices, precedence (W1) and the
   empty value (W2), grouped, together with the user, and record the
   decisions.
3. Build one reading logic that the start and the check share (F1).
4. Build the check's output so that each value shows its source (F2),
   a lower rank is reported when a setting is given twice (F5), and secrets
   never appear (F3).
5. Verify that the check binds nothing (F4): run it twice in a row while
   the service is running.
