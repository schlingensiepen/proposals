---
type: Proposal Concept
description: The problem of statements about a revision other than the one running, the concept of a stamp that travels inside the artifact and is checked before publishing, and a short three-step procedure.
---

# The concept

## Problem

An artifact that does not say which revision it was built from makes every statement about its behavior unverifiable. In an implementation by the author, a container ran for days at a revision seven changes behind the one being talked about, and there was no way to notice: the image carried no stamp, and the identifier in its name was a branch name, which does not change. For days, every statement that something had been "tested" was a statement about something other than what was running.

On top of that, "I have deployed" and "it is running" are two different statements: an unchanged name pulls no new image. That came to light only once the artifact could report its build; on its first use, the report at once caught a container that had taken an unchanged tag from the cache.

## The concept

The revision a build came from travels inside the artifact itself. The stamp belongs in the image because only the image reliably reaches whoever asks; a companion file stays behind, and a name lies. The report belongs in the artifact, not in the delivery, because only what is running can say what is running. And a smoke test in the build checks the one layer no test of the source sees: that what is published starts and identifies itself.

For any concept that demands one deliverable, this is what makes the demand checkable: without the stamp, "one deliverable" is a claim that cannot be verified on the target machine.

**Load-bearing structure.** One rule; F1 carries the concept alone.

## The procedure

Not yet complete; three steps so far.

**1 · The version command reports release and build.** When the commands of the artifact are set up, the version includes the revision the artifact was built from. (F1)

**2 · Stamp, and check before publishing.** The build stamps the source revision into the artifact, and the build pipeline starts the deliverable before publishing it and requires that it reports this revision. *Result:* the build pipeline with its smoke test. (F1)

**3 · The first look after deploying.** The version command against the running instance reports the revision that was meant to be deployed. *Result:* what is running is demonstrably what was built. (F1)
