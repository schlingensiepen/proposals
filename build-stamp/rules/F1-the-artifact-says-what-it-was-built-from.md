---
type: Proposal Rule
description: The build stamps the source revision into the artifact itself; the version command reports it against the running instance; the build pipeline publishes nothing that does not report exactly the revision built; a local build says "unknown".
number: F1
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F1 · The artifact says which revision it was built from

**Applies to:** builder · operator.

**What holds.** The build stamps the **source revision**, the revision it was built from, into the artifact itself, not into a companion file and not into the name of the deliverable; a release number alone is not enough, because it does not move between two builds. The version command reports this stamp, and it is run against the running instance: in a container as a one-off invocation inside the running container, on the host as the same binary; on the host it reports the revision of the installed binary, and only the restart after installation guarantees that the running process is the same. The build pipeline starts the built deliverable **before** publishing it and requires that the report returns exactly the revision built; a deliverable that cannot do that is not published. A locally built artifact says "unknown" rather than inventing a revision. This rule does not require a report over the network: the readiness point still says nothing but "running"; where a network surface reports the revision, it is authenticated. Of this, the stamp and the smoke test are evidenced; the report through the version command is **not**: the author's implementation today reports the stamp only through an authenticated management surface on the network, and its version command reports only the package version. At this point the rule goes beyond the evidenced implementation (see the [evidence](../evidence.md)).

**Why.** An artifact that cannot say what it was built from makes every statement about its behavior unverifiable. In the implementation, a container ran for days at a revision seven changes behind the one being talked about, and there was no way to notice: the image carried no stamp, and the identifier in its name was a branch name, which does not change. For days, statements about "tested" were statements about something other than what was running. The stamp belongs in the image because only the image reliably reaches whoever asks; a companion file stays behind, a name lies. And the report belongs in the artifact, not in the delivery, because "I have deployed" and "it is running" are two different statements: an unchanged name pulls no new image, and exactly that was noticed only once the report existed. Finally, the smoke test in the build checks the one layer no test of the source sees: that what is published starts and identifies itself.

**Obvious and rejected.** *The release number as the report*: does not move between two builds; two images with the same number can behave differently. *The revision in the name of the deliverable*: a branch name is a pointer, not a revision; and the namespaces of version control and registry do not coincide. *A companion file beside the artifact*: does not travel along; the question arises on the target machine, where only the artifact is. *The revision in the readiness point*: turns it into a report on internals that anyone can read; the boundary between an operational point and a management surface would be crossed.

**How you notice it is violated.** Someone asks how a statement about what is running came about, and nobody can answer; or a deployment reports success, and the service behaves as before.
