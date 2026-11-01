---
type: Proposal Concept
description: The problem of credentials in a medium that logs, shares and can be steered by foreign text, the concept of a double cut of narrow capability tokens and fixed handling rules, its load-bearing structure, and the procedure for operator and agent.
---

# The concept

## Problem

A classic API client is a program with a config file: the token sits in one place, flows through one code path, and who gets to see it is decided by file permissions. An agent is different. Its "working memory" is a context window that is logged, summarized, shared, and processed further; its "configuration" is often conversation; and it processes foreign content — mails, web pages, documents — that can move it to act through mere text (prompt injection). A token that stands in the chat history is no longer a secret but an inhabitant of a searchable, exfiltratable medium.

The usual reactions fall short: "agents get no tokens" prevents useful automation; "we trust the agent" ignores that the agent itself can be attacked. What is needed is a design that reckons with partial failure.

Three roles carry the problem: **the operator** of a service issues accesses and must be able to reel them back in. **The agent** stores and uses them in an environment that was never meant for keeping secrets. **The user** decides who may do what — and should be able to do so without ever taking a secret into their hands.

## The concept

The core is a **double cut**. On the service side, tokens are cut as narrow **capabilities**: a token *is* the permission for exactly one ability at exactly one place — there is no parameter with which the caller chooses the effective scope, and hence no authorization code that could be wrong. Whoever needs two targets gets two tokens. On the agent side, a fixed **handling code** applies: received once, stored outside the conversation text, never quoted again, used exclusively against the place it was issued for.

The two sides are joined at the **setup** — the moment when the token wants to travel from the operator to the agent without passing through human hands. For this, the concept recommends open **self-registration** with a **display string**: the agent introduces itself, receives a token and a pronounceable, non-secret handle, and the user grants the rights to the handle — never to the secret. The rest is lifecycle: one-time display at creation, storage by the fixed ranking of `where-a-secret-comes-from`, rotation as routine, revocation that hits exactly one purpose, expiry for everything unconfirmed and unused.

The concept is deliberately general: it presupposes no particular type of service. A worked application, a construction principle that makes these rules binding for self-describing web services, is shown in the evidence.

**Map of the rules.** Twelve rules in three blocks along the lifecycle. **F1 to F4** are the *cut on the service side*: what a token embodies, how it comes into being, where its ability lives, and which surfaces the service hardens for it. **F5 to F9** are the *lifecycle*: handover, storage, binding, rotation and revocation, expiry. **F10 to F12** are *agent behavior and its delivery*: the never-rules, the way of speaking about tokens, and the duty to ship all of this with the service. Two rules are explicitly **recommendations** (F5 self-registration as the recommended setup solution, F11 display string); the rest is binding once the concept is adopted. Every rule names who it applies to.

**Load-bearing structure.** F1 carries the cut; F2 to F4 are its concretization for issuance and surfaces — without server-side creation with one-time display (F2) and without scope-in-the-token (F3), all that remains of the narrow cut is the intention, and F4 closes the leak surfaces through which even the narrowest token escapes. F5 joins the service side and the agent side at the setup. F6 to F9 are the lifecycle: storage (F6) and binding (F7) protect the living token, rotation and revocation (F8) and expiry (F9) contain the dying one. F10 and F11 are the agent behavior; F12 makes it deliverable — if F12 falls, all agent rules remain hope. If F1 falls (at a legacy API with an inherited password, say), F5 to F11 remain fully in force as the handling code for every secret: the concept breaks apart into two halves, each load-bearing on its own, and that is exactly the intention.

## The procedure

**Operator (build order).**

1. Design the capability cut: list the narrow abilities your service offers; one token type per ability and place. (F1)
2. Build issuance: server-side generation, prefix, hash storage, short ID, one-time display, display string. (F2, F11)
3. Harden the surfaces: bearer-header-only transport, short-ID logging, 401/404 semantics without an existence oracle. (F4)
4. Build the handover: self-registration with flavors and throttling, or document your second channel; define and document expiry deadlines and thresholds. (F5, F9)
5. Make rotation and revocation single commands; put every expiry date into calendar and monitoring. (F8)
6. Write the agent-facing rules (storage ranking, binding, never-rules, display-string usage) into your service documentation. (F12)

**Agent (receiving to using).**

1. Receive the token via the service's self-registration if offered (send a short self-introduction naming your user and purpose); otherwise ask the user to provide it over a channel outside the chat, ideally into the announced environment variable. (F5, F6)
2. Store it by the ranking; if the token came with a display string, report only that to the user ("please grant <display-string> the … rights"). (F6, F11)
3. Use it only against its place of issue; confirm with the user before actions that take effect, as the service's documentation demands. (F7)
4. On loss or suspicion: ask the operator side for rotation, never for re-display. (F2, F8)
5. When the user ends the purpose: have the token revoked; expect that exactly one ability disappears and nothing else. (F1, F8)
