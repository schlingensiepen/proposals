---
type: Proposal Rule
description: Files generated from other files carry a visible note that they are generated and name their leading source; changes happen only at the source; generation runs automatically so that deviations show.
number: F7
binding: must
role: independent
applies_to: [writer, maintainer]
---

# F7 · What is generated is marked as generated, with its source named

**Applies to:** writer · maintainer.

**What holds.** Where files are generated from other files (split decision documents, directories, overviews), they carry a visible note that they are generated, and name the leading source. Changes happen exclusively at the source; generation runs automatically along with it, so that deviations show.

**Why.** Generated files look like written ones, and that is exactly the trap: somebody changes the copy, the change disappears on the next run, and nobody understands why. The note costs one line and answers the question before it arises. Importantly, this rule does not violate F1: the generated file is fully readable; the note on the source is a maintenance note, not a prerequisite for understanding.

**Obvious and rejected.** *Not checking in what is generated*: then it is missing for whoever has only the repository; for a self-contained repository, availability without build tools is precisely the point. *Maintaining generated files by hand*: drifts at once. *Documenting only in the build configuration that something is generated*: nobody who has the file in front of them finds it.

**How you notice it is violated.** Somebody changes a generated file, and the change disappears.
