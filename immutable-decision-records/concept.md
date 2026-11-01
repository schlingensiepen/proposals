---
type: Proposal Concept
description: The problem of amended decision records, the lost reason and the invisible switch; the concept of immutable records replaced as a whole and kept at both ends; its load-bearing structure.
---

# The concept

Decision records get amended in day-to-day work, and that is exactly how they lose their value: in the end you see only the latest state and no longer know what applied when, or why. Immutability plus complete replacement restores both, today's state *and* the path to it, at the price of more documents and some redundancy.

## Problem

"I decided a year ago to build it this way. That I decided it, I still know. What I had against the obvious alternative back then, I no longer know, so I think it through again, arrive with the same head at mostly the same result, just three days later. And when I open the record, there is a sentence someone corrected at some point. Whether that was the original decision or a later repair, I cannot see."

That is the normal case with amended decision records. Two distinct kinds of damage hide in it:

- **The lost reason.** A decision without its rejected alternatives is an opinion. It is abandoned at the first pushback: by a new colleague, by an assistant system, by one's own self two years on.
- **The invisible switch.** When a record is corrected in one place, the result is a text that *looks* like one decision but in truth layers two decisions from different years on top of each other. Nobody can see where the seam runs.

The second damage is the more expensive one, because it does not announce itself. A missing reason is annoying; an invisible seam leads someone to implement a long-revised decision while believing they follow the current state.

## The concept

Every decision gets its own short record with a sequential number. While it is a draft, it is worked on. The moment it is accepted, its content becomes **immutable**: nobody edits the prose again. Whoever wants to revise the decision writes a **new** record that answers the question completely afresh, with its own rationale, its own rejected alternatives, its own title, and records in the head of both documents which replaces which. The old one stays, is marked as superseded, and is never deleted. An index across the whole body shows at a glance what applies and what is merely history. Numbers are never reused, not even after a draft was discarded. This leaves exactly two questions answerable at any time: *what applies today?* and *what applied back then, and why was it given up?*

**Load-bearing structure.** F2 carries everything: if immutability falls, F3, F4, F5 and F6 lose their purpose; they only govern what a replacement looks like, and without immutability there would be no replacements, only corrections. F1 is the precondition of F2: only what applies as a whole can be immutable. F7 and F8 stand independently; they make sense even for an amended body of records and can be adopted separately. Whoever wants to take away one rule takes F7.

## The procedure

The concept prescribes no tool and no file format. The steps for recording a new decision, revising an existing one and migrating a collection file into individual records are in the agent guide ([`AGENT.md`](./AGENT.md)); the rules in [`rules/`](./rules/F1-one-record-one-decision.md) say what must hold at the end of each.
