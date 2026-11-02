---
type: Proposal Evidence
description: Where the concept has held; its origin as the generalization of a sister concept, the worked application to self-describing services, a secret doctrine in operation, a reference implementation in progress, and a fictitious walk-through.
sources:
  - id: dennis-van-horn-1966
    url: https://doi.org/10.1145/365230.365252
    title: Programming Semantics for Multiprogrammed Computations
  - id: hardy-confused-deputy
    url: https://css.csail.mit.edu/6.858/2015/readings/confused-deputy.html
    title: The Confused Deputy (or why capabilities might have been invented)
  - id: owasp-llm01
    url: https://genai.owasp.org/llmrisk/llm01-prompt-injection/
    title: OWASP Gen AI Security Project, LLM01:2025 Prompt Injection
---

# Evidence

This section separates what is in operation, what is in progress, and what serves illustration only. Every piece of evidence is presented in the same grid.

## Origin

The concept generalizes what first arose in `self-describing-services` and applies to every service that agents consume. The problem analysis, the design rules that are now F2 to F4 and the security table come from an earlier draft of August 2026 that was withdrawn before publication.

It transfers an old idea into a new situation: capabilities [^dennis-van-horn-1966] [^hardy-confused-deputy] bind the authorization to an unforgeable artifact instead of to the identity of the caller — exactly right for a consumer whose "identity" is an attackable context window [^owasp-llm01]. What is new is not the model but the handling code around it: one-time display, ranking, never-rules, display string — the concessions to a medium that was never meant for keeping secrets.

## Evidence 1 — Worked application: the self-describing services

**What it is.** `self-describing-services` applies this concept to one type of service, self-describing web services.

**What it demonstrates.** That the rules of this concept can be applied to a concrete type of service. It demonstrates applicability, not operational experience.

**Status, honestly.** Applicability on paper; for a service specified this way, see Evidence 3.

## Evidence 2 — Precursor in operation: the secret doctrine of the wiki toolkit

**What it is.** A toolkit of the author's that makes the wiki platform Wiki.js usable for agents and enforces a strict secret doctrine in doing so: environment variable before file, file only as a hardened pass-through, the secret never appears in an output; several wiki instances are served at the same time, each with its own access. Its repository is not publicly accessible.

**What it demonstrates.** The storage ranking (F6) and the binding to the place (F7) are practicable under load — with this toolkit a wiki with 140,000 nodes was built up without a secret ever turning up in transcripts or logs. Not demonstrated is the token cut: the platform uses inherited API keys — exactly the case in which only the handling half of this concept applies.

**Status, honestly.** In operation and in daily use; a precursor, not a model.

## Evidence 3 — Reference implementation in progress: the mail filing service

**What it is.** A service of the author's, specified according to the sister concept from Evidence 1 and thereby according to this concept: narrowly cut tokens (one token = one target), display strings, issuance on the operator's side; the self-registration is envisaged as an expansion stage. Its repository is not publicly accessible.

**What it demonstrates.** So far, that the rules can be specified in full for a real service. Operational experience is not demonstrated: the implementation by a development agent is under way; a completion report was still outstanding when this version was written and will be added here.

**Status, honestly.** Specification complete, implementation running, no consumer in everyday use.

## Worked example (entirely fictitious): the traffic-report access

What follows is **not evidence** but illustration — all values are fictional. A user has their agent set up a traffic service. The agent calls the service's self-registration and introduces itself: "Agent of one commuter; purpose: report and retrieve traffic jams on the commute." The answer carries the flag **by-request**, a token with the service's prefix, and the display string `kluvmoth`. The agent stores the token in the environment variable the service proposes for it, and tells its user only: "Please give the access **kluvmoth** the reporting rights." The user activates `kluvmoth` server-side — the secret has never touched the chat (F5, F6, F11). Months later the operator reads their token list: next to `kluvmoth` is the self-introduction, and they still know who got access there and why (F5). When the user changes their commute, they revoke `kluvmoth` — one ability lapses, everything else keeps running (F1, F8). A second token that they applied for "for later" a year ago and never activated has long since expired (F9).

[^dennis-van-horn-1966]: https://doi.org/10.1145/365230.365252
[^hardy-confused-deputy]: https://css.csail.mit.edu/6.858/2015/readings/confused-deputy.html
[^owasp-llm01]: https://genai.owasp.org/llmrisk/llm01-prompt-injection/
