---
type: Proposal Rule
description: The header of every shipped deployment file answers what the operator needs before pulling, which image and what kind of name, whether login is needed, the steps to pull, start and check, what travels in clear text, which variables pass through and where state lives.
number: F6
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F6 · The header of the deployment file: what the operator reads before pulling

**Applies to:** builder · operator.

**What holds.** That the deliverable comes with a deployment file versioned with the source, which **names a published image** instead of building one on the target machine, and that a local build is a named exception, is a rule of `service-as-single-artifact`; it is used here, not justified. What is stated here is what the **header** of this file has to answer, so that an operator who has nothing else knows what they are pulling: **which image** it names and **what kind of name** it is — moving (follows the builder; a fresh pull fetches the new state) or immutable (pins a state; a change is an edit to the file) — and how to switch from one kind to the other; **whether a login** to the registry is needed and, if not, that this has been measured (F5); **the steps** to pull, start and check, in the order in which they are to be carried out, with the explicit pull step before the start and the check call after it (F7); **what travels over the wire in this variant** and for whom it is therefore not meant; **which variables** the operator may set, and that the file passes them through instead of defining them empty; and **where the state lives** and that it outlives the container. If several deployment files are shipped because several operating situations are served, each carries this header. The file that is the exception and builds locally says so in its first line and names the reason — and the reason is maintained with the file so that it does not stand there longer than it holds. The form of the header is a pattern; what is binding is the list of what it answers.

**Why.** The operator has nothing but this file and the image. No script talks to them, no person stands next to them, and the builder's documentation is probably not open in front of them. What is not in the header, they guess — which name to take, whether they have to log in, whether they have to pull, how they know it worked. The header is also the only place where F3, F4, F5 and F7 can reach the operator: the computed names, the promise `latest`, the measured visibility and the check after the rollout only help them if the file tells them. And stating the kind of name has a reason of its own: a moving name obliges the operator to pull and check (F7), an immutable one to edit the file for every state; anyone who does not know which one they have does the wrong one of the two.

**Considered and rejected.** *Putting the instructions in a document of their own* — it does not travel; the file travels. *An example without a name ("enter your image here")* — shifts the decision onto the person who does not know the choice. *Naming the name and keeping quiet about its kind* — the operator takes a moving name for fixed or a fixed one for following; the reference does that today (see the evidence). *One deployment file for all operating situations, with switches* — then the price of a variant does not stand where it is chosen, and the variable that chooses the variant has no header. *Always the immutable name* — right for anyone who wants to pin a state, wrong as the default for anyone who wants to follow the builder: they would have to edit the file for every state, and they do not.

**How you notice it is violated.** An operator asks which image to take, whether they have to log in, whether they have to pull or how to check that it runs — or they consider a state current because they did not know that their name moves.
