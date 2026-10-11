---
type: Proposal Rule
description: Service and commands are the same executable artifact, binary, package or directory, with the mode chosen at invocation; a minimum set of modes; no separate administration tool.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · One artifact, several modes

**Applies to:** builder.

**What holds.** Service and commands are the same executable artifact; the mode is chosen at invocation. The minimum set is: the **service mode**; a **check command** that assesses the configuration without starting; a **state command** that shows what is pending; **configuration commands** for setting up and changing; and the **setup command**, which also issues the access. Further process kinds (worker, scheduler) are further modes of the same artifact. A separate administration tool is not built. "One artifact" is a unit that is built, and replaced, as a whole: a binary, a package or a directory; which form it takes in the concrete case is fixed in step 9 of the procedure. A directory artifact may have several entry points as long as it is one build from one source.

**Why.** The commands share everything essential with the service: configuration format, storage layout, validation rules, data schema, version. A second tool is a second implementation of this knowledge — and two implementations of the same knowledge drift apart as soon as one of them changes. Within the same artifact they cannot. The minimum set is named explicitly, because without it the cut would remain an attitude: every service that runs longer than a week needs these five things.

**Obvious and rejected.** *A separate little administration tool* — allegedly cleaner, in fact a second configuration parser with a version number of its own. *An administration web UI* — convenient, but a permanently open attack surface with a login of its own (F7). *Scripts that write directly into the store* — bypass every check the service applies when writing, and know the structure only by hearsay.

**How you notice it is violated.** Administration tool and service carry different version numbers — or a new configuration field has to be carried over in two places.
