---
type: Proposal Rule
description: Delivery form is part of the description: prose steps default to text/plain, the exact type only on explicit Accept, Vary: Accept; a switch in the form of the answers is announced or avoided.
number: F16
binding: must
role: load-bearing
applies_to: [builder, façade operator]
---

# F16 · The delivery form belongs to the description: weakest reader, announced switch

**Applies to:** builder (in a retrofit: façade operator).

**What holds.** Media type, character set and headers with which entry page, usage description and interface description are delivered are part of the description and are aligned with the **weakest intended reader**, not with the most exact truth about the content. Concretely: by default the prose steps are delivered as `text/plain`, also to callers that send `Accept: */*` or no `Accept` header at all, and only whoever names a more exact type explicitly (`text/markdown`, say) gets it; the bytes are the same in both cases, and every such response carries `Vary: Accept`. The same rule applies to the **form of the answers**: the description says in which form the service answers, and where a consumer reads the description as prose but the answers come in another mode of operation, it announces the switch, or better still avoids it, by having the service deliver the same data in the form the caller asks for via `Accept`, generated from the same data.

**Why.** A description the reader cannot accept is none: the service has then answered correctly and is still unusable, and precisely for the kind of caller the self-description is built for. A tool that sends `*/*` and then discards the answer by its type is the normal case, not the exception; a rule that reacts only to one particular desired type does not reach it. The default has to be reversed, not merely supplemented by a special case. And whoever reads the entry page as prose expects the continuation in the same mode of operation: a switch in the middle of the flow is a hurdle the builder does not see, because he knows both forms. Several delivery forms are not a second source in the sense of F1 as long as they arise at runtime from the same data; a second, separately maintained version would be.

**Obvious and rejected.** *Delivering the exact media type because it is true* (`text/markdown` with `nosniff`): true and unusable; browsers download instead of displaying, fetch tools discard. *Reacting only to `text/html`*: does not help the caller with `*/*`, and that caller is the observed case. *Placing a typeset HTML version for humans next to it*: a second representation, maintained separately, that drifts apart; humans have the interactive contract view of their framework. *Only mentioning the switch of mode in the description*: the follow-up question comes anyway, because the reader skips the sentence but not the answer; in the observed case it came a second time after only the text had been supplemented.

**How you notice it is violated.** The service answers with `200` and complete content, and callers still report "technical failure"; every fetch checked by hand gets through, and the search for the fault turns to certificates, protocol versions and blocklists. Or a reader asks after the first data call whether text should not have come there.
