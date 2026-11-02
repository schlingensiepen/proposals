---
type: Proposal Rule
description: Image names are produced by a computation that knows the registry's naming rules, never copied verbatim from version control, and every build gets at least one immutable name that carries the source revision beside the moving ones.
number: F3
binding: must
role: load-bearing
applies_to: [builder]
---

# F3 · Names are computed, not formed — and every build gets an immutable one

**Applies to:** builder.

**What holds.** The names under which an image is published are produced by a **computation** that knows the rules of the target namespace and turns values from version control into valid names; no name is taken verbatim from a branch name, a version label or any other value. Every build receives at least one **immutable** name that carries the source revision, and beside it the **moving** names that follow a branch or a version. In the pattern: `sha-<full source revision>` as the immutable name; `<branch name, sanitized>` and `<version>` as moving ones; the words are interchangeable, the split into two kinds is not.

**Why.** The namespaces of version control and of the registry do not coincide: a branch name may contain characters that a name for an image may not — the slash is the common case. A build that inserts the branch name verbatim works on every branch that happens to keep the rule and fails without a result on the first one that breaks it; on the default branch it never shows, because its name keeps the rule. That is the kind of fault only the branch that breaks the rule reveals, and then as a riddle after a successful build step. The immutable name has a second reason: an operator who wants to pin a state needs a name that denotes exactly one build; the digest does that, but is known only after the build and is not human-readable. A name derived from the source revision can be read off the build log, written into a deployment file and compared with what the running instance reports (F7).

**Considered and rejected.** *The name is the branch name* — the incident. *Only a version tag, set by hand* — gets forgotten; between two versions nothing moves, and two builds carry the same name. *Only the digest as the immutable identifier* — correct, but not human-readable and known only after the build; it is no use for the comparison with the build stamp, because the running instance reports its source revision, not its digest. *Cleaning up the version-control namespace ("no slashes in branch names")* — a convention against a tool; it holds exactly until someone does not know it, and the computation is cheaper.

**How you notice it is violated.** A build fails with a message about an invalid reference format after the build step itself has succeeded — or the question "which name belongs to exactly this build?" has only a moving name as its answer.
