---
type: Proposal Agent Guide
name: two-repo-submodule-pattern
description: Self-contained guide for an agent that separates a project into a work repository and a product repository; the steps with the duties of adoption, the rules, an acceptance checklist, common mistakes and a self-check.
---

# Agent guide

## Purpose

You are separating what arises while something is being made from what is published: you set up a work repository and a product repository, connect them, and carry things across from one to the other. Or you are reviewing an existing setup for a polished history, a reference into a repository nobody can see, or a submodule reference that has stopped moving. This guide is complete on its own except for the comprehension check of the review (F7), which comes from the imported concept `proven-from-outside`; ask the user for that bundle if you do not have it. Rules are named in parentheses as provenance.

## Procedure

**0 · Ask early whether there should be two repositories, and check whether the separation pays off.** Ask the user at the start of the setup whether they want two repositories at all. Are there really two kinds of content, a product for others and a genesis (drafts, decision logs, rejected paths, session notes)? Does the permission management allow one repository to be held back while the other is shown? If everything is published anyway, or the genesis is itself to be published, or the drafts are themselves what others receive, the separation is effort without return; stop and say so. Without a second repository the rules below on handover, review and release still hold, only without a repository boundary. *Result:* a reasoned decision.

**1 · Duty of adoption: decide the allocation.** Derive from the usual allocation and the purpose of the project what belongs on which side, and agree it with the user. The usual allocation, as a starting point: the workshop holds the knowledge base as the source of truth, the registers and the linked working documents (specification, architecture, handover states for agents); in the product, code, tests and build arise directly and are not derived, while the self-contained documents for the outside (user, developer and operations documentation, README) are derived; the handover is triggered by a release or on request, generates anew in full, and nothing is synchronized back. Then build the handover accordingly: what it takes over, what it turns from linked into self-contained (references to the workshop and register identifiers removed, reasons written out), and what it leaves outside (lifecycle data of the registers, internals). Record the allocation where every later handover will meet it. *Result:* the recorded allocation and a handover built for it.

**2 · Set up the two repositories.** One work repository for the genesis, whose history is a diary and may be untidy; one product repository that contains only what a consumer receives, whose history is a publication history. Do not smooth the one and do not fill the other with working traffic. *Result:* two repositories with their purposes written into their entry-point documents. (F1)

**3 · Fix the direction.** The work repository embeds the product repository, never the reverse. The product contains no reference to the workshop, not even "for those interested". *Result:* a product that can be cloned and read on its own. (F2)

**4 · Determine the tool situation, and let it decide the form.** Ask: can the tools through which the work is done write submodule references? Interfaces that edit repositories only through file operations cannot, because a submodule reference is an entry type of its own, not file content. If they can, embed the product repository as a submodule in a subdirectory of the work repository (F4). If they cannot, check the product repository out in a subdirectory of the work repository, put that subdirectory on the ignore list of the work repository, and write out in the entry-point documents of both sides that the two belong together (F5). The condition decides, not preference; do not create a submodule whose reference you will not be able to advance. *Result:* the embedding, with the reason for the form recorded. (F4, F5)

**5 · Duty of adoption: agree who may write where without announcement.** Where several participants work on the same repositories, a person and an agent, say, write down explicitly who may write in which repository without asking. What has proven itself: the repository assigned to somebody as their workshop they write freely; any other only after announcing what will be written. The answer depends on who contributes; the concept only demands that it is made explicitly and written down. *Result:* the written write rule. (F6)

**6 · Carry things across deliberately.** Every handover from workshop to product is a deliberate act: decide what goes across, and take over the statement together with its reason, never a link to a workshop document. Do not update what was taken over piecemeal; when it needs to change, generate it anew in full from the source. The product must be fully understandable without any access to the work repository. *Result:* a publication that stands on its own. (F3)

**7 · Offer the review, emphatically.** Before every handover, offer a full review and make clear that you recommend it; or ask once whether it should always run, and treat a yes as a standing approval that the user can switch off in the individual case. Have it done by two independent reviewers of different models, neither of them the model that built. Review, incorporate, review again until it is settled; after at most three rounds without settling, stop and give the human a convergence report: what converged, what did not, what they have to decide. Never approve silently. Ask in every review whether what is taken over contains names, access paths or statements about third parties. For the self-contained documents, have a fresh agent without access to the workshop play the target reader: it retells the content, walks through it and lists where it had to guess; treat these places as defects. *Result:* a reviewed handover, or a convergence report. (F7)

**8 · Record the release.** Every release reads in full "state x of ⟨artifact⟩, released for purpose Z" and is bound to a versioned, frozen state. Record the release for delivery in the workshop as a decision naming the state handed over, and set a release tag in the product repository. In the two-roots form this is the only record of which product state belongs to which working state; in the submodule form it comes in addition to the reference. *Result:* a release decision and a tag, and somebody who can say when, what and why something was published. (F8)

**9 · Keep the reference moving.** In the submodule form, advance the submodule reference with every working state that rests on a new product state, and publish the product state before the working state that refers to it; otherwise a clone of the work repository meets a reference to a state it cannot fetch. A reference that has stood still for months while the product moved on is a claim, not a reference. *Result:* a work history in which every working state names its product state. (F4)

## Rules

MUST: genesis and product in separate repositories, neither history smoothed nor bloated (F1) · the work repository embeds the product repository (F2) · statement and reason taken over at every handover, the product understandable on its own, what was taken over generated anew instead of updated piecemeal (F3) · two roots with an ignore rule when the tools cannot write submodule references (F5) · an explicit rule for who may write where without announcement (F6) · a review offered emphatically before every handover, two reviewers of other models than the builder, at most three rounds, then a convergence report (F7) · every release with state and purpose, a decision in the workshop and a tag in the product (F8).

SHOULD: the submodule form wherever the tools allow it (F4).

NEVER: rewriting history before a publication (F1) · a reference from the product to the workshop (F2) · links from the product into workshop documents, automatic synchronization from workshop to product, synchronizing back from product to workshop (F3) · a submodule whose reference is maintained by hand or not at all (F5) · writing into someone else's repository without announcement (F6) · a review by the model that built, or a silent approval (F7) · a release without a frozen state or without a purpose (F8).

## Acceptance checklist

- [ ] The user was asked early whether there should be two repositories; the allocation of workshop, product and handover is recorded.
- [ ] Two repositories with two purposes; nobody had to rewrite history before a publication, and nobody hesitates to record something in the workshop. (F1)
- [ ] The product contains no reference that an outsider cannot follow. (F2)
- [ ] No document in the product refers to one that is only in the workshop; what was taken over is generated anew, not edited in the product. (F3)
- [ ] The form of the embedding is recorded together with the tool situation that decided it. (F4, F5)
- [ ] Submodule form: the reference has moved with the product. (F4)
- [ ] Two-roots form: the embedded directory is on the ignore list. (F5)
- [ ] The write rule is written down; nobody has discovered a change in the product they knew nothing about. (F6)
- [ ] Before every handover a review was offered; it was done by two reviewers of other models than the builder and ended settled or with a convergence report. (F7)
- [ ] Every release names state and purpose, is recorded as a decision in the workshop and as a tag in the product; for every publication you can say when and why. (F8)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| One repository, history smoothed before publication | Two repositories; the genesis keeps its dead ends, the product gets a clean history of its own (F1) |
| Two branches in one repository instead of two repositories | Branches share access rights; use two repositories (F1) |
| The allocation left implicit, so every handover decides anew what goes across | Decide and record the allocation at setup, starting from the usual one |
| A link in the product to the workshop "for those interested" | Remove it; anyone who wants to show the genesis publishes it as product (F2) |
| A workshop document linked or included instead of carried over | Take over its statement and its reason in the product's own text (F3) |
| A document in the product corrected by hand after the source changed | Generate it anew in full from the source (F3) |
| Automatic synchronization from workshop to product | A deliberate handover with review (F3, F7) |
| The builder reviews its own handover | Two reviewers of other models (F7) |
| The review rounds run out and the handover goes ahead anyway | A convergence report to the human; they decide (F7) |
| "Released" without saying which state and for what | "State x of the artifact, released for purpose Z", decision and tag (F8) |
| A submodule created although the tools cannot advance its reference | Two roots with an ignore rule; a stale reference pretends to a precision it does not have (F5) |
| The separation given up because the connection does not work cleanly | Keep the separation; it holds even without a reference (F5) |
| The embedded directory committed into the outer repository | Ignore rule in the two-roots form (F5) |
| The work state with a new submodule reference published before the product state it points to | Publish the product state first; otherwise clones of the work repository break (F4) |
| The agent writes into the product without saying what | Announce first, then write (F6) |

## Self-check

Before you report completion: can you name the work repository and the product repository and what each contains according to the recorded allocation, the form of the embedding and the tool situation that decided it, the recorded write rule, how the review before a handover runs, and where each release is recorded with its state and purpose? Can somebody clone the product and understand it without the workshop? If any of these is missing, you are not done.
