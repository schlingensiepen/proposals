---
type: Proposal Rule
description: After every rollout, success is a call against the running instance that reports its build state, compared with the state meant to be rolled out; anyone who delivers through a moving name pulls explicitly before the start.
number: F7
binding: must
role: load-bearing
applies_to: [operator, builder]
---

# F7 · Rolled out is not running: success is a call that reports the state

**Applies to:** operator · builder.

**What holds.** After every rollout, success is not the end of the rollout procedure but a **call against the running instance** that reports its build state, compared with the state that was meant to be rolled out — the source revision from the build log or the immutable name (F3). The precondition is that the image carries its source revision and the running service reports it; that is a rule of the imported concept `build-stamp` and is used here, not justified. Anyone who delivers through a moving name **pulls explicitly before the start**, because the container runtime serves a name it knows from its local cache; the deployment file names the pull step and the check call (F6). A date, a green mark in the build service, a successful restart of the container are no proof of what is running. In the author's delivery, the check after the rollout is a practice and is in no shipped file: the header names neither the call nor what to compare it with. F7 goes beyond the proven implementation at this point (see the evidence).

**Why.** The container runtime does not pull an image again whose name it knows — it has one under that name, and a moving name does not tell it that something behind it has changed. In the author's delivery, after a build and a "deploy", the old state kept running, twice in a row; it was noticed only because the image had recently begun to report its build state. Without the stamp, an entire acceptance run would have gone against the wrong state, and nobody would have known — just as earlier a container had run for days seven changes behind the state that was being discussed. "I have rolled out" is a statement about an action; "it is running" is a statement about the result; and only the second counts.

**Considered and rejected.** *Looking at the date of the container or of the build log* — the incident; both say that something happened, not what is running. *Letting the runtime always pull ("pull always")* — right as the default wherever the runtime offers it, and still no substitute for the check: a pull can fail, and the old container keeps running. *Shipping only immutable names* — solves the cache problem, but moves the action to the operator, who would have to edit the file for every state (F6). *The version number as the answer* — does not move between two builds; two images with the same number can behave differently.

**How you notice it is violated.** A rollout reports success, and the service behaves as before — or an acceptance run applies to a state that was not the one rolled out, and this shows only when someone asks how a statement about the running instance came about.
