---
type: Proposal Rule
description: Reading calls return the same objects under the same identifiers with the same facts as the main surface; the form follows Accept, text by default and JSON on request, with Vary Accept; refusals stay in the one error format.
number: F4
binding: must
role: load-bearing
applies_to: [builder]
---

# F4 · Reading mirrors the main surface; the form follows the caller

**What holds.** The reading calls of the second surface return the same
objects under the same identifiers with the same facts as the main
surface; there is no second truth about a file or an operation. The
**form** of the answer follows the caller's `Accept` header: whoever
names `application/json` gets the main surface's structure unchanged;
whoever says nothing or `*/*` gets the same facts as `text/plain`, line
by line `name: value`, generated from the same data and therefore never
diverging. Every such answer carries `Vary: Accept`. Refusals are
excluded: they stay in the service's one error format, regardless of
the mode of interaction.

**Why.** Whoever uses this surface has read its description as prose;
switching the mode of interaction in the middle of the flow is a hurdle
for exactly the tools the surface exists for. In the observed case a
reader asked after the first data call whether text should not have come
back there, and asked a second time after only the description had been
amended and the behavior left as it was. The structure stays reachable,
because otherwise the surface would no longer be a pass-through.
Generating both forms from the same data is what makes two forms
affordable: a text form written by hand per answer would be a second
description of the same facts. And refusals stay in one format, because
a caller must be able to recognize them without guessing whether that
was an error.

**Obvious and rejected.** *Always JSON, "because the main surface does
it"*: the switch of mode that cost the two questions. *Always text*:
then nobody who does want to process can get the structure, and the
surface would have one capability less than the main surface. *A
separate, nicer text form per answer*: the second source in one's own
house. *Refusals as text too*: then the caller must recognize from the
wording that something went wrong.

**How you notice it is violated.** A caller asks whether an answer
should not have been text; or the same file shows different facts on the
two surfaces.
