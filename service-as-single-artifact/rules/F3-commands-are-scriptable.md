---
type: Proposal Rule
description: Every command ends with a fixed, documented exit code, offers machine-readable output where it lists, and never demands interactive input it cannot take as an argument.
number: F3
binding: must
role: independent
applies_to: [builder]
---

# F3 · Commands are scriptable

**Applies to:** builder.

**What holds.** Every command ends with an exit code whose meaning is fixed and documented; at a minimum it distinguishes: all good · runtime error · wrong invocation · configuration invalid. Commands that list or show state additionally offer a machine-readable output. No command demands an interactive input that it does not also accept as an argument.

**Why.** Administration without a surface has to be automatable — otherwise the second tool comes about through the back door: as a collection of scripts that take apart text outputs and break silently at every format change. Exit codes are the cheapest contract a program can offer, and the only one a deployment chain understands without assistance.

**Obvious and rejected.** *Output text only ("one can search it, after all")* — makes every improvement of the format a break of someone else's automation, and a soundless one at that. *Interactive prompts without a non-interactive path* — then the command hangs in every unattended environment. *Everything over one collective exit code* — the difference between "invoked wrongly" and "configuration broken" is exactly the one an operator needs at three in the morning.

**How you notice it is violated.** In the deployment chain there is a text search on a command output — or a command waits for an input in an unattended environment.
