---
type: Proposal Rule
description: What gets deployed is exactly what is versioned in the repository, an HTML file and the files beside it; no bundling, transpiling or minifying.
number: F1
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F1 · One artifact, not a build result

**Applies to:** builder, operator.

**What holds.** What gets deployed is exactly what is versioned in the repository: an HTML file and the files beside it. There is no step that turns sources into something else — no bundling, no transpiling, no minifying.

**Why.** A build step is a state between source and deployment. It can go stale, produce a different result on another machine, and be forgotten at deployment time — and each of those problems surfaces in operation, not while building. Without it, the question "is the same thing running there as here?" is answered by comparing files. And rolling back is copying back, with nothing having to be regenerated.

**Considered and rejected.** *Bundle and minify, "that is what one does"* — pays off for large applications with many dependencies; for an application running off a handful of files you buy milliseconds of load time with a toolchain that wants maintaining. *A build step "only" for the third-party libraries* — then it exists, so the target system will need it eventually.

**How you notice it is violated.** Somebody asks "which version is running?", and the answer requires executing a tool.
