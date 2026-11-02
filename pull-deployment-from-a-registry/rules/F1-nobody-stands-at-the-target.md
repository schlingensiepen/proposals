---
type: Proposal Rule
description: In a pull deployment nobody stands at the target, so every assurance a push script would provide at the target is placed either before publishing, in the build pipeline, or in the artifact and its deployment file.
number: F1
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F1 · In a pull, nobody stands at the target: every check is placed before publishing or in the artifact

**Applies to:** builder · operator.

**What holds.** The deployment consists of two roles and six actions: the builder **builds** the image, **checks** it and **publishes** it to a registry; the operator **pulls** it from there, **starts** it and **checks** what is running. No action writes into the target machine from outside. There is no script that places anything at the target, no deployer who knows the target machine, and no checker who looks at the target from outside after the transfer. Therefore every assurance that a push script would provide at the target is placed at exactly one of two places: **before publishing**, in the build pipeline (F2 to F5), or **in the artifact and its deployment file** (F6, F7), so that the operator can verify it with what they hold — the image, the deployment file and a call against what is running.

**Why.** In a push, the channel into the target exists before anything happens, and the script can look at the end at what it has done. In a pull, this channel does not exist, and the builder usually does not even know who pulls, or when. What the builder has not checked before publishing, nobody checks any more — until the operator notices it in operation, where fault-finding is most expensive and the follow-up question takes a detour through people who do not know the deployment. And what the artifact does not say about itself, the operator cannot know, because nobody stands next to them. The rule is therefore not a matter of tidiness but the answer to the question of where a check is possible at all.

**Considered and rejected.** *An agent on the target machine that reports back* — a second channel with its own secret and its own upkeep; and for an operator who is not the builder, a back channel they do not want and should not have to consent to. *Retrofitting push "just for the check"* — then it is no longer a pull; you carry both blocks of cost and lose the advantage that the operator does not have to give anyone access. *Doing without the check, "the operator will get in touch"* — that is the state before the smoke test, and the operator does get in touch: with fault-finding in the wrong direction whose cause was in the deployment.

**How you notice it is violated.** A fault that starting the image in the build environment would have shown is reported by the operator — or the builder asks the operator what is running on their side, and the operator cannot say.
