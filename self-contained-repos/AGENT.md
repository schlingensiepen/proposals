---
type: Proposal Agent Guide
name: self-contained-repos
description: Self-contained guide for an agent that makes a repository self-contained or reviews one for prerequisites that point outside; procedure with the duties of adoption, rules, acceptance checklist, common mistakes and self-check.
---

# Agent guide

## Purpose

You are making a repository self-contained, so that a human or an agent who opens it for the first time can work from it alone, or you are reviewing a repository for knowledge that lives only outside it. This guide is complete on its own; rules are named in parentheses as provenance only.

The check question behind everything: **does one still understand and follow the repository when every external link is dead?** Links are evidence or deepening, never a prerequisite.

## Procedure

**0 · Check the preconditions.** The repository must be the basis of the work (not a code dump beside a wiki or ticket system where the work actually happens), there must be recurring activities on it, and somebody must maintain it. If one of these is missing, say so to the user before you continue; the concept then has little to hold on to.

**1 · Name the tasks and write the entry point.** Find the two or three genuinely different tasks that arise on this repository (for example: change something, use something, look something up). Write exactly one entry file that does not explain the undertaking but names these tasks and points per task to the instruction for it. Put it where the widespread agent tools look first; where several such places are customary, the other files contain nothing but a pointer to this one. Keep it short enough to be read before the work. If you find more than three tasks, you are probably listing topics; merge them. (F2)

**2 · Write one task definition per recurring activity.** For each activity that recurs: what to do, in which order, how to tell it is finished. Describe the activity, not the undertaking, the tool or the motivation. Do not merge them into one large guide and do not leave them in tickets. (F3)

**3 · Make every instruction self-contained.** Every rule and every check question is written out in the instruction itself. A reference to a number or section of another document may stand only in parentheses as provenance, and following the instruction must not need it. Write the instruction as commands, not as a copy of the reasoning. (F4)

**4 · Bring decisions in with their reasons.** Wherever a document relies on a decision, take over the statement and the reasons, and where it carries the matter, the rejected alternatives with the reason for each rejection. A link to the decision log may remain as a maintenance note. When you inline: take the matter, not the context (no internal names, customer data or access paths); use your own words, not the wording of outside documentation; put the evidence in the sources. Where the document is generated from the source that holds the decision, do not correct the reasons in the copy; generate it anew in full from the source. (F5)

**5 · Write down the pairings.** **Duty of adoption:** list the pairs of places in this repository that say the same thing in different forms and must change together (typically discussion paper and instruction, instruction and checklist, document and register entry). The list depends on the repository; record it where every change will meet it, and handle every pairing in the same change as the change itself. (F6)

**6 · Mark what is generated.** Every file produced from other files carries a visible note that it is generated and names its leading source. Changes happen only at the source; the generation runs automatically so that deviations show. Check generated files in, so that they are available without build tools. (F7)

**7 · Apply the dead-link test.** Go through every document as if every link to the outside were dead. Wherever a reader would have to follow a link to keep working, bring the knowledge in (step 4) or name the place explicitly as one where self-containment is not reached (for instance because a quality system requires the statement to exist only once, or because the knowledge belongs to somebody else and a summary is not enough). Do not claim self-containment where it does not hold. Reading it yourself is not enough, because you read with the very knowledge the links are meant to replace: have a reader without inside knowledge, for instance a fresh agent with nothing but the repository and a task, work from the repository and report where it had to follow a link or guess; treat these places as defects. This acceptance by a reader from outside is imported from `proven-from-outside`. (F1)

**8 · Protect the instructions.** Files that an agent follows unchanged have effect. Tell the user that changes to them need the same review and history as code changes, and before a repository is published, review everything you brought in for confidential material.

## Rules

MUST: every document followable with all external links dead; outside knowledge brought in with statement and reasons (F1) · exactly one entry file that branches to the two or three tasks; other entry files only point to it (F2) · one task definition per recurring activity (F3) · instructions with every rule written out, numbers only as provenance in parentheses (F4) · decisions with statement and reasons, rejected alternatives where they carry (F5) · pairings written down and handled in the same change (F6) · generated files marked, source named, changed only at the source (F7).

NEVER: a link as a prerequisite for understanding (F1) · a user manual as the entry point for contributors, or several equal entry files (F2) · a checklist that hangs on numbers of another document (F4) · "see decision 12" without statement and reasons (F5) · a hand edit of a generated file (F7).

## Acceptance checklist

- [ ] Every document still understandable and followable with every external link dead; places where this is not achievable are named as such; a reader without inside knowledge has worked from the repository and its findings are handled. (F1)
- [ ] Exactly one entry file, short, naming two or three tasks with one instruction each; further entry files only point to it. (F2)
- [ ] Every recurring activity has its own instruction with order and a recognizable end. (F3)
- [ ] No instruction needs a second document to be followed. (F4)
- [ ] Every decision a document relies on stands there with its reasons; nobody has to fetch another document to answer "why?". (F5)
- [ ] The list of pairings exists and is where every change will meet it. (F6)
- [ ] Every generated file carries the note and names its source; generation runs automatically. (F7)
- [ ] Inlined material reviewed for confidential content and outside wording.

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Relying on links to internal systems because they "always work" | Bring the statement and reasons in; links outlive neither moves nor tool changes (F1) |
| A link checker in the build taken as proof | It finds dead links, not living links that are prerequisites; apply the check question (F1) |
| The writer applies the dead-link test to their own text | They cannot forget what they know; have a reader without inside knowledge do it (F1) |
| The user manual doubles as contributor entry | One entry point for contributors that branches; the manual serves users of the product (F2) |
| A separate full entry file per agent tool | One leads, the others only point to it (F2) |
| One large contributor guide | One instruction per recurring activity, reached through the entry point (F3) |
| A short checklist pointing to rule numbers | Write every point out; numbers only in parentheses as provenance (F4) |
| Generating the instruction mechanically from the discussion paper | Write it as commands; a mechanical derivation yields commands that argue or arguments that command (F4) |
| Only the statement of a decision, without reasons | Add the reasons and, where they carry, the rejected alternatives (F5) |
| The decision log put into the repository as the answer | Keep it as history; bring the decision into the working document (F5) |
| Trusting care to keep copies in line | Name the pairings and handle them in the same change (F6) |
| Not checking in generated files | Check them in, marked, so they are there without build tools (F7) |

## Self-check

Before you report completion: can you name the entry file and the tasks it branches to, the instruction for each recurring activity, the list of pairings, every generated file with its source, and every place where self-containment is knowingly not reached? Then read the repository once as if every external link were dead. If you had to follow a link to keep working, you are not done.
