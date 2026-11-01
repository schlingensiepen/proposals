---
type: Proposal Concept
description: The problem of keeping the making of something and its published product in one place, the concept of two repositories with two histories connected in one direction, the two forms of the embedding, the load-bearing structure, and the usual allocation of workshop and product as a starting point.
---

# The concept

**In one sentence.** What arises while something is being made (drafts, logs, rejected paths, session notes) and what is published in the end belong in two different repositories with two different histories; they are connected as a submodule or, where the tools do not allow that, as two independent roots with an ignore rule.

**In short.** Whoever publishes a product wants a history worth showing: clean changes, no dead ends, no internal notes. Whoever works on it needs the opposite: every dead end, every reason, every intermediate version. Both in one repository leads either to a polished history or to a publication that reveals more than intended. Two repositories solve this, and the only interesting question left is how to connect them without getting two truths.

## Problem

A collaboration, here between the author and an AI agent, produces two things. One is the **product**: code, documentation, everything somebody else is meant to use. The other is the **genesis**: draft versions, decision logs with date and course, rejected proposals with their reasons, notes from working sessions, research that did not make it into the result.

Put both into one repository and you have to decide which side suffers. Publish everything, and the genesis goes public along with it, including dead ends, half-finished thoughts and occasionally things that were meant only internally. Publish nothing, and the product cannot be shared. And the most popular way out, smoothing the history before publication, destroys exactly what makes the genesis valuable: the reasons and the rejected paths.

Add to this an observation from working with agents: the genesis is not waste but **working material**. An agent that continues a project needs the decision log more urgently than a human does, because it remembers nothing. Deleting the genesis means taking the memory away from whoever works on it next.

Three roles carry the problem. **The author** wants to keep both but show only one. **The consumer** of the product wants to see a history that says something about the product and not about afternoons. **The successor**, often an agent, needs the genesis complete, or starts over from scratch.

## The concept

Two repositories, two purposes, two histories. The **work repository** is the place of the genesis: concepts, short specifications, decision logs, session states, research, plans. Its history may be untidy, for it is a diary. The **product repository** contains only the product, in the form in which somebody else receives it; its history is a publication history.

They are connected so that **one direction leads**: the work repository embeds the product repository, not the other way round. Whoever works has both in front of them; whoever wants only the product clones only that and sees nothing of the workshop. There are two forms for the embedding.

The **submodule form** is the complete one: the product repository lies as a submodule in a subdirectory of the work repository. Its advantage is an exact reference: the work repository records on which state of the product a given working state was based. So for every point in the genesis it can be reconstructed which product belonged to it.

The **two-roots form** is the frugal one: two independent repositories, one checked out in a subdirectory of the other, and that subdirectory is on the ignore list of the outer one. There is then no recorded reference any more; in return, everything works with tools that can only write files. Exactly this case occurred in practice, and it is the reason this form stands here on equal footing (F5; the episode is told in the evidence).

Above both stands a rule that matters more than the choice between them: **nothing moves unnoticed from one to the other.** The handover from workshop to product is a deliberate act, and whatever is taken over from the genesis is not linked but carried over with its statement and its reason; otherwise the published product hangs on a repository nobody can see.

What is taken over is not maintained by hand afterwards: when needed, it is generated anew in full from the source, so the product can lag behind as a whole but cannot contradict itself. Before every handover a review is offered, by reviewers other than the one that built (F7), and every release names the state it releases and the purpose it releases it for, recorded as a decision in the workshop and as a release tag in the product (F8).

**Load-bearing structure.** F1 carries everything: without the separation, F2 to F8 are pointless. F2 gives the separation its direction, F3 makes it durable; without the copy rule, the product ends up hanging on the workshop after all. F4 and F5 are alternatives to each other, not complements; which one applies is decided by the tool situation, and both carry F1 equally well. F6 is independent and holds wherever several people work on the same repositories. F7 and F8 are independent of the form: F7 gives the deliberate handover of F3 its check, F8 records what was released for what; in the two-roots form F8 takes the place of the reference that F4 would record.

## The usual allocation: a starting point, not a rule

The rules govern *how* the handover happens, not *what* belongs across. As a starting point that the user adapts, this allocation has proven itself:

* **Workshop:** the knowledge base as the source of truth, the registers, and the linked working documents (specification, architecture, handover states for agents).
* **Product:** code, tests and build arise **directly in the product** and are not derived; the decisions about them are still recorded in the workshop. Added to this are the self-contained documents for the outside (user, developer and operations documentation, README); these are derived.
* **Handover:** triggered by the release for delivery or on request. It generates anew in full and turns linked versions into self-contained ones: references to the workshop and register identifiers are removed, reasons are written out. Lifecycle data of the registers and internals stay outside. The release goes through the review (F7); synchronizing back from the product into the workshop is forbidden.

Two axes must not be confused here. **Original or derived** occurs on both sides; **linked or self-contained** changes only at the handover. Derived documents in the workshop, such as a specification for readers with access to the repository, stay short and linked; only the documents beyond the handover are self-contained. Without a second repository the same rules hold without a repository boundary. The agent guide makes the allocation a duty of adoption: it is decided early, from this starting point and the purpose of the project, and recorded.
