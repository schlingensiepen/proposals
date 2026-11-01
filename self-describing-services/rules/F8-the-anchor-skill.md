---
type: Proposal Rule
description: The service provides a storable skill that contains no knowledge, only purpose, target and links, and the entry page actively offers it.
number: F8
binding: must
role: independent
applies_to: [builder]
---

# F8 · The anchor skill: a shell of links, actively offered

**Applies to:** builder.

**What holds.** The service provides a classic, storable skill that contains **no knowledge** but exclusively forwards: purpose of the service · target (base URL) · link to the self-description · one (purpose, link) pair per part description. The package is divided into files along the thematic division (F3). And the entry page **actively offers this skill** — a sentence of the form "You can save or install this service as a skill: <link>", together with the reference — because what is merely listed somewhere falls into oblivion.

**Why.** Persistence is useful for *triggering*, not for *knowing*: a stored skill triggers when the user says something pertinent — but only an empty shell cannot go stale; what can go stale is solely its references, when the parts are recut (the scope names the cost). The anchor skill anchors triggering in the usual agent infrastructure while itself remaining a forwarding layer made of links; resolution happens only at the time of use. The active offer on the entry page makes the proven flow reliable: "install `<url>` for me" suffices, and the agent finds the skill because the entry page holds it out to him.

**Obvious and rejected.** *Storing complete copies of the instructions in the skill* — drift; precisely the second source that F1 forbids. *Offering nothing storable at all* — then nothing triggers, and the user has to name the URL every time. *Calling the thing a "façade"* — the term is taken by the retrofit (F15); one word with two meanings would be exactly the confusion the terms section is meant to prevent.

**How you notice it is violated.** A locally stored skill describes yesterday's interface — or users use the service for months without knowing that the skill exists.
