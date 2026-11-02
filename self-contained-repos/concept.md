---
type: Proposal Concept
description: The problem of the repository whose reasons and expectations live elsewhere, the concept of bringing knowledge in instead of linking it with its three components and its load-bearing structure, and the order in which the rules are applied.
---

# The concept

**In one sentence.** A repository is self-contained when someone who opens it for the first time, human or agent, can work without asking back and without outside documents: an entry point branches to the actual tasks, every decision is in it with its reasons in full text, and the check rule is whether everything still holds when every external link is dead.

**Short thesis.** Knowledge about an undertaking naturally scatters: into the ticket system, the wiki, chat histories, people's heads. Anyone who then opens the repository finds code and a user manual, but not why something is the way it is, and not what is expected of them. For a human this is a nuisance; for an agent it is the end of its ability to work, because it cannot ask down the hall. Self-contained means: everything needed to understand and contribute is in the repository, and what comes from outside is brought in with its statement and reasons instead of being linked.

## Problem

The author works with AI agents on several repositories and has made an observation along the way that holds just as much for humans, only less painfully: **an agent cannot ask.** It has no hallway, no colleague and no memory of the last project. What is not in the repository does not exist for it, and what it does not find, it invents, with the best of intentions and full conviction.

The usual state of a repository is the opposite. The code is in it, and usually a user manual too. But the *why* is elsewhere: in decision records of a ticket system, on a wiki page, in a design document on a shared drive, in a chat history nobody can find any more. And the expectation toward anyone who contributes is written down nowhere; it is experience. Anyone who joins asks questions; anyone who cannot ask guesses.

This has three visible consequences. **Rejected paths are proposed again**, because nobody remembers that they were already examined and rejected for a good reason, and the reason is missing exactly when somebody questions the decision. **Links rot**: a link to an internal document is a shortcut today and a dead end in two years; the knowledge behind it is then lost, although the repository still looks complete. And **every newcomer costs the same onboarding**, because it is written down nowhere and happens orally every time.

Three roles carry the problem. **The writer**, who does not notice while writing what they silently presuppose. **The successor**, who months later finds decisions whose reasons nobody knows any more. And **the agent**, who without context ends up in the same position as a newcomer on the first day, except that it cannot ask, only guess.

## The concept

The answer is a shift: **knowledge is brought in, not linked.** A self-contained repository contains not only what the system does, but also why it is the way it is and what is expected of anyone who works on it. Three components carry this.

First, an **entry point for contributors** that does not explain but branches. In practice a repository has two or three genuinely different tasks, such as changing something, using something, looking something up, and the entry point names them and sends the arriving person to exactly the instruction that belongs to their task. It is a signpost, not a handbook: short enough that everyone reads it before they start.

Second, the **inlining rule for decisions**: when a document in the repository relies on a decision made elsewhere, **statement and reasons** are taken into the document. A link to the decision log is at most a maintenance note, never a prerequisite for understanding. Redundancy is deliberately accepted; it is cheaper than the coupling.

Third, the **check rule** that turns the intention into a verifiable statement: *Does one still understand and follow the repository when every external link is dead?* This question can actually be asked during review, and it fires on every linked prerequisite for understanding. It is best asked by somebody who does not already know the answers: the writer reads with the very knowledge the links are meant to replace. That a document meant to work without inside knowledge is accepted only once a reader without that knowledge has worked with it is imported from `proven-from-outside`.

Added to this is an insight that only arose in practice: **an instruction for agents must not hang on numbers from another document.** A checklist that points to "see rule 3" forces the agent back to the full text after all; then the separate file would be pointless. References to the document of origin may therefore appear only as a provenance note in parentheses; the rule itself must be written out.

**Load-bearing structure.** F1 carries everything: without the check rule, F4 and F5 are only good will. F2 and F3 are the structure in which self-containment gets a form at all: without an entry point nobody finds the instruction, without instructions there is nothing to find. F5 is the rule with the greatest yield and the most resistance, because it demands writing whose benefit only becomes visible later. F6 is the price for F1 and F4 together; if it is not paid, self-containment turns into contradiction. F7 is independent and holds also for repositories that adopt nothing else from this concept.

## The procedure

This concept has no sequence of steps of its own; it says what must hold, not in which order one gets there. Anyone who makes an existing repository self-contained applies the rules in the order of the load-bearing structure: first the structure (an entry point that branches, F2; one task definition per recurring activity, F3), then the content of those instructions (written out, F4; decisions with their reasons, F5), then maintenance (the pairings that change together, F6; generated files marked, F7), and at the end, and at every later review, the check question of F1 applied to every document, by a reader without inside knowledge. The agent guide spells this out as a procedure with checklists.
