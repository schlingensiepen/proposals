---
type: Proposal Rule
description: The name an operator gets who names none, latest in the common pattern, is a promise and is written only by the build of the default branch, or not at all.
number: F4
binding: must
role: load-bearing
applies_to: [builder]
---

# F4 · `latest` is a promise and follows the default branch alone

**Applies to:** builder.

**What holds.** The name an operator gets who names none — `latest` in the common pattern — is written **exclusively by the build of the default branch**, by no other branch and by no manually triggered build of another branch. It means: the state the builder recommends to anyone who knows nothing else. If the builder cannot say that of the default branch — because what they would recommend is not there — the name is **not written** rather than made to mean something else; the deployment file then names a different name (F6).

**Why.** An operator who pulls without a name gets this one — and usually does not know that they have made a choice by doing so. If every build writes the name, they get whatever was built last, by chance: a working branch, an experiment, a state that nobody ever recommended to anyone. That is not disorder but a false promise to the person who knows least. And the promise is only as good as its source: a default branch that lags behind the work for weeks turns `latest` into "the oldest thing we still have", and the rule "only from the default branch" is then kept and helps nobody (see the evidence).

**Considered and rejected.** *Writing it from every build* — the incident; inconspicuous on the default branch, a slipped-in state on the first working branch. *Not writing it at all* — defensible if you say so: an operator who then pulls without a name gets an error instead of a wrong state, and that is better; but it moves the question into the deployment file, which then has to name a name. *Pointing at the latest version instead of the default branch* — the better meaning as soon as there are versions; then "the newest version tag" takes the place of the default branch, and the rule applies accordingly: exactly one named source for the name, never "whatever was built last".

**How you notice it is violated.** An operator who pulls without a name has a state the builder never recommended — or `latest` is older than what the builder considers current.
