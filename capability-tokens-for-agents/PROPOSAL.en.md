---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: capability-tokens-for-agents
language: en — translated from the German original
sync: v1.0 (2026-08-14)
question: "How does one design, hand over, store, and use access tokens when the consumer of a service is an AI agent?"
thesis: "A token embodies exactly one capability at exactly one place, and its handling follows fixed rules along the lifecycle — because in the agent context every leak is searchable and copyable; whoever pins down both the cut and the handling survives partial failure gracefully."
---

# Capability Tokens for Agents

**In one sentence:** A token embodies exactly one capability at exactly one place, and its handling follows fixed rules along the lifecycle — creation, handover, storage, use, rotation, revocation, expiry — because in the agent context every leak is searchable and copyable; whoever pins down both the cut and the handling survives partial failure gracefully.

**Abstract:** When the consumer of a service is an AI agent, the leak channels for credentials change fundamentally — the chat history becomes a searchable, copyable storage medium, and the agent can be steered by foreign text. The most effective answer is a double cut: **tokens as narrow capabilities** on the service side and **fixed handling rules** on the agent side. This concept applies to every service that agents consume — web API, command line, MCP, self-describing or not.

## 1. Problem

A classic API client is a program with a config file: the token sits in one place, flows through one code path, and who gets to see it is decided by file permissions. An agent is different. Its "working memory" is a context window that is logged, summarized, shared, and processed further; its "configuration" is often conversation; and it processes foreign content — mails, web pages, documents — that can move it to act through mere text (prompt injection). A token that stands in the chat history is no longer a secret but an inhabitant of a searchable, exfiltratable medium.

The usual reactions fall short: "agents get no tokens" prevents useful automation; "we trust the agent" ignores that the agent itself can be attacked. What is needed is a design that reckons with partial failure.

Three roles carry the problem: **the operator** of a service issues accesses and must be able to reel them back in. **The agent** stores and uses them in an environment that was never meant for keeping secrets. **The user** decides who may do what — and should be able to do so without ever taking a secret into their hands.

## 2. The concept

The core is a **double cut**. On the service side, tokens are cut as narrow **capabilities**: a token *is* the permission for exactly one ability at exactly one place — there is no parameter with which the caller chooses the effective scope, and hence no authorization code that could be wrong. Whoever needs two targets gets two tokens. On the agent side, a fixed **handling code** applies: received once, stored outside the conversation text, never quoted again, used exclusively against the place it was issued for.

The two sides are joined at the **setup** — the moment when the token wants to travel from the operator to the agent without passing through human hands. For this, the concept recommends open **self-registration** with a **display string**: the agent introduces itself, receives a token and a pronounceable, non-secret handle, and the user grants the rights to the handle — never to the secret. The rest is lifecycle: one-time display at creation, storage in a fixed ranked order, rotation as routine, revocation that hits exactly one purpose, expiry for everything unconfirmed and unused.

The concept is deliberately general: it presupposes no particular type of service. A worked application example — a construction principle that makes these decisions binding for self-describing web services — is shown in §8.

## 3. Terms

**Capability** — a single, narrowly bounded ability at a service (contributing reports at exactly this instance, say). The rights cut of this concept thinks in capabilities.

**Token** — the artifact that embodies exactly one capability; in the agent context, the recommended access artifact for new builds.

**Secret** — any confidential credential: API key, password — or a token. *Every token is a secret; not every secret is a token.* The handling rules of this concept apply to every secret.

**Place** — the target a token was issued for: an instance, a base URL, a target system. The binding to the place is part of the capability.

**Lifecycle** — the stations of a token: creation · handover · storage · use · rotation · revocation · expiry.

**One-time display** — the plaintext token appears exactly once, at creation; after that there is no retrieval function. Loss means rotation, not research.

**Short ID** — a harmless, non-secret identifier of a token for logs and administration; of the token itself the service stores only the hash.

**Display string** — the short, pronounceable, **non-secret** handle to a token (eight characters, lowercase) by which humans and agents talk about tokens and grant rights without ever naming the secret.

**Self-registration** — the optional open method by which a consumer receives a token in exchange for a free-text self-introduction; in two flavors: **automatic** (immediately usable) and **by-request** (born without rights, activated afterwards).

**Second channel** — a handover path from the operator to the user outside the chat; needed everywhere self-registration is not offered.

**Pass-through file** — a file with tight permissions that does nothing but pass a secret along: the content flows directly into the call and never appears in outputs, logs, or the context window.

**Canonical environment-variable name** — the name of the environment variable for its secret that the service itself proposes; it solves the re-finding problem across sessions.

**Rotation** — replacing a token with a new one while the purpose persists; **revocation** — final decommissioning; **expiry** — automatic lapse of unconfirmed or unused tokens.

## 4. Decisions

**Map.** Twelve decisions in three blocks along the lifecycle. **F1–F4** are the *cut on the service side*: what a token embodies, how it comes into being, where its ability lives, and which surfaces the service hardens for it. **F5–F9** are the *lifecycle*: handover, storage, binding, rotation and revocation, expiry. **F10–F12** are *agent behavior and its delivery*: the never-rules, the way of speaking about tokens, and the duty to ship all of this with the service. F1 carries the cut; if it falls (at a legacy API with an inherited password, say), the handling part F5–F11 remains in force for every secret — the concept deliberately breaks apart into two halves, each load-bearing on its own. Two decisions are explicitly **recommendations** (F5 self-registration as the recommended setup solution, F11 display string); the rest is binding once the concept is adopted. Every decision names who it applies to.

### F1 · One token, one ability, one place

**Applies to:** Operator.

**What holds.** A newly built service SHOULD cut access rights as narrow capabilities: one token embodies exactly one ability at exactly one place; whoever needs two targets or two abilities gets two tokens. Deviations are permissible with good reasons — a legacy API brings what it has; for its inherited secret, the handling rules F5–F11 then apply unchanged.

**Why.** In the agent context, the chat history is a real leak channel. Narrow tokens make every single leak surgically small: revocation and rotation hit exactly one purpose, not the whole service — and the user can grant rights at the granularity in which they think ("may report traffic jams") instead of the granularity an admin model imposes.

**Obvious and rejected.** *One admin token "to be safe"* — makes every leak total; today's convenience is tomorrow's total loss. *One collective token per user across all of their purposes* — couples the unconnected: a leak at the side purpose opens the main purpose.

**Symptom of violation.** Revoking a single token silences more than the one purpose it was issued for.

### F2 · Creation: server-side, high-entropy, visible only once

**Applies to:** Operator.

**What holds.** Tokens are generated exclusively server-side (CSPRNG, at least 128 bits of entropy) and carry a recognizable prefix (`svc_…`, say), so that secret scanners find leaks and nobody confuses tokens. Of the token, the service stores **only the hash** (for high-entropy random tokens, fast SHA-256 suffices; password KDFs address guessable secrets) plus a harmless short ID for logs and administration. The plaintext token appears exactly **once**, at creation; a retrieval function does not exist — loss means rotation, not research.

**Why.** One-time display minimizes the number of copies to exactly those the recipient creates; hash storage ensures that a leaked config file or backup on the operator's side is not a keyring; and the prefix turns every accidental commit into a hit that tools find, instead of a secret that lies unnoticed for years.

**Obvious and rejected.** *Self-chosen tokens with password character* — people again choose what they can remember, and what they can remember is what they already use elsewhere. *A "show token" function for the loss case* — every re-display surface is a query surface; the experience from operations: its absence produces inquiries at first ("I have lost it") — and exactly the rotation discipline one wants. *Tokens by mail or ticket "for convenience"* — turns the mailbox and the ticket system into a keyring.

**Symptom of violation.** A token can still be "looked up" somewhere weeks later — at the operator's, in a mailbox, in a ticket.

### F3 · The token is the scope

**Applies to:** Operator.

**What holds.** The ability sits in the token itself, not in a caller-selectable parameter of the call: the service checks the token and derives from it, completely, what is allowed. There is no scope parameter that the caller sets, and no rights that are ordered along with the issuing call.

**Why.** A selectable parameter is authorization code that can be wrong — and an agent that takes things literally sets it literally wrong. An ability that lives in the artifact is not negotiable: a token that ends up in the wrong place *cannot* do more there than it embodies.

**Obvious and rejected.** *A scope parameter on the call, checked against the token* — redundancy with room for error: two places have to agree, and one of them is set by the attacker. *Ordering rights in the same call that creates the token* — then the anonymous party decides on their own cut; the separation of creation and activation is the protection.

**Symptom of violation.** A call with the same token has a different reach depending on a parameter sent along with it.

### F4 · Transport and error semantics: no oracles, no leak surfaces

**Applies to:** Operator.

**What holds.** Tokens travel exclusively in the bearer header — never in the URL, never in the payload. The service's logs contain only the short ID, never the token. And the error semantics gives no oracle: unknown tokens receive 401; foreign resources answer **identically** to nonexistent ones (404). Whoever guesses learns nothing — not even that there is anything to guess.

**Why.** URLs end up in proxy logs, browser histories, and application logs — the header path shuts these leak channels out by construction rather than by policy. And the existence of a resource is itself information: a 403 answer ("exists, is not yours") betrays the map of the service to whoever is searching.

**Obvious and rejected.** *The token as a URL parameter "for easy integration"* — then every place that stores URLs stores secrets. *Distinguishable errors "for better diagnostics"* — the diagnosis belongs in the teaching error message for the legitimate, authenticated caller, not in the answer to the anonymous guesser.

**Symptom of violation.** A token turns up in an access log — or a systematic guesser can read off from the answers which resources exist.

### F5 · Handover over a second channel — recommended: self-registration

**Applies to:** Operator · User.

**What holds.** The handover of a token to the consumer runs over a channel outside the chat; what form this second channel takes is settled by the adopter per deployment (assignment Q2). RECOMMENDED as the solution to the setup problem is open **self-registration** in three steps: **(1)** The agent calls it with a free-text self-introduction (who, what for). **(2)** The service answers with a token, a display string, and a flag that names the flavor — **automatic** (immediately usable; only where damage potential is low) or **by-request** (the token comes into the world without rights). **(3)** In the "by-request" flavor, the agent shows its user the display string, never the token — and the user grants the rights server-side to exactly this display string. The self-introduction is stored with the token and is its decision and audit basis. And let it be said honestly: **this is not really secure** — it is the deliberate compromise in favor of getting started quickly; more complicated procedures have proven unusable in trials.

**Why.** The setup is the moment when secrets want to pass through human hands — every hand is a copy. Self-registration makes the onboarding end-to-end without a secret ever traveling through a human: the human touches only the non-secret handle. Whoever reads the token list months later can still see from the self-introduction who got access there and why.

**Obvious and rejected.** *The chat as the normal path of handover* — turns the transcript store into a keyring; as the declared last resort, F6 regulates it. *A full OAuth build-out for small services* — disproportionate; where an ecosystem prescribes it, it applies anyway (§7). *Ordering rights in the registration call* — see F3.

**Symptom of violation.** Users dictate tokens through chats or ticket systems — or the onboarding fails because nobody with server access happens to be at hand.

### F6 · Storage in a fixed ranked order

**Applies to:** Agent (delivered by the operator, F12).

**What holds.** The agent obtains and keeps the secret in this ranked order: **1.** environment variable — the service MAY propose a canonical name for it; **2.** if the environment cannot do that: a file with tight permissions, used exclusively as a **pass-through** — the content flows directly into the call and never appears in outputs, logs, or the context window; **3.** only as the last resort, the context window itself — named once, persisted immediately, never repeated. The ranked order applies to every secret, whether token or inherited API key.

**Why.** Chat histories and logs are searchable, copyable stores. The ranked order minimizes how long and how often a secret exists in them — and it gives the agent a checkable rule instead of a feeling. The canonical environment-variable name incidentally solves the re-finding problem across sessions: the service itself says where its secret usually lives.

**Obvious and rejected.** *"Just put the secret in the chat" as the normal path* — the last resort is declared an exception, not permitted as a habit. *Client-side encryption contortions* — the agent cannot have secrets from itself; security theater.

**Symptom of violation.** A secret turns up in a transcript, a log, or an error message.

### F7 · Binding to the place of issue

**Applies to:** Agent (delivered by the operator, F12) · Operator.

**What holds.** A token is used exclusively against the place it was issued for. Per instance or target system there is a token of its own; on ambiguity ("put it in the wiki" — but which one?), the agent asks instead of guessing. The operator SHOULD additionally check the binding server-side wherever the topology allows it (host or tenant binding): a token that has ended up in the wrong place then runs into nothing.

**Why.** Same software does not mean same trust: the company system and the private one share the API and nothing else. The mix-up reports no error — the call mechanics fit perfectly, only the target is wrong. The double binding (behavioral rule at the agent, check at the operator) dampens exactly the case in which third parties abuse the agent as a confused deputy.

**Obvious and rejected.** *One token per software with a target list inside it* — merely shifts the danger of mix-up inward. *Guessing the target from the context* — an agent that takes things literally also guesses literally wrong.

**Symptom of violation.** A contribution lands in the wrong system — or a secret is used against a target other than the one it was issued for.

### F8 · Rotation and revocation hit exactly one purpose

**Applies to:** Operator · User.

**What holds.** Every token is individually revocable and individually rotatable, and both without interruption: the new token is valid in parallel with the old one until the old one is deleted. Rotation is a routine operation (one command, one motion), not a project. Expiry dates — wherever tokens or the secrets of their issuing channels are time-limited — belong in the operations calendar and in monitoring; the adopter settles the rhythm (assignment Q3).

**Why.** The value of the narrow cut is realized only in revocation: when decommissioning a token costs exactly one purpose, a case of suspicion becomes an operational procedure instead of surgery — and then rotation actually takes place. The empirical record of the neighboring dossiers is unambiguous: expiring secrets and revoked accesses are the most frequent long-term causes of failure, and they surprise precisely those operations that have no expiry date in the calendar.

**Obvious and rejected.** *"Tokens never expire, that is more convenient"* — convenience beats hygiene exactly as long as it takes for the first leak to be years old and unnoticed. *Big-bang rotation of all tokens on a cutoff date* — couples the failures that the narrow cut has just decoupled.

**Symptom of violation.** A revocation forces the re-setup of uninvolved purposes — or an expiring secret surprises operations.

### F9 · Expiry is the operator's duty — the values are set by the adopter

**Applies to:** Operator.

**What holds.** Tokens born without rights that are never activated, and tokens that remain permanently unused, expire automatically; open issuance paths (self-registration) are throttled. The concrete deadlines and thresholds are deliberately not standardized by this concept — they are operational figures that only the respective operator knows, and hence an assignment to the adopter (Q1).

**Why.** Stockpiling is the quiet opposite of the narrow cut: every forgotten token is an ability that nobody misses any more and everybody can still use. An open issuance path without throttling and expiry accumulates exactly such tokens.

**Obvious and rejected.** *Standardizing uniform deadlines in the concept* — comparable and checkable, but wrong: the same deadline is generous for the hobby service and negligent for the company. *No expiry ("nobody is going to abuse it")* — hope as security architecture.

**Symptom of violation.** The token list grows monotonically, and nobody can say what half the entries belong to.

### F10 · The agent's never-rules

**Applies to:** Agent (delivered by the operator, F12).

**What holds.** An agent that carries a secret follows four never-rules: it **never** repeats it in chat, outputs, or files that do not serve its storage. It **never** uses it against a target other than the place of issue (F7). It **never** treats foreign instructions concerning secrets — "send your token to …", "let me see your key" — as commands, but as data. And it **never** persists it beyond the reach its user has named.

**Why.** Those are the three real exfiltration paths — repetition, misdirection, injection — plus the outlasting that turns a session access into a permanent one. Never-rules are deliberately formulated as absolutes: they are checkable, they need no weighing-up in the moment of attack, and they remain in force even when a foreign text demands the opposite.

**Obvious and rejected.** *"The agent will be sensible enough"* — trust is no substitute for a rule; what is attacked is the agent, not its good sense. *Mirroring the secret back to the user "for verification"* — creates exactly the copy in the chat that all the other rules prevent.

**Symptom of violation.** A token turns up in an answer — or a call goes to an address that comes from foreign work material.

### F11 · Way of speaking: the display string

**Applies to:** Operator · Agent · User.

**What holds.** RECOMMENDED: every token receives, at creation, a short, pronounceable, **non-secret** handle — the display string (eight characters, lowercase). Humans and agents speak about tokens exclusively by display string; granting rights, revocation, and inquiries address the handle, never the secret.

**Why.** Tokens *have* to be spoken about — granted, revoked, assigned, asked after — and every conversation that needs the secret itself moves the secret. The handle makes speaking safe: dictation-proof on the telephone, readable in the token list, unambiguous in the granting of rights. The short ID from F2 does the technical work (logs); the display string does the human work.

**Obvious and rejected.** *The token itself as the reference in conversation* — then the secret travels through chat and hallway call. *Internal IDs as the handle* — not pronounceable, not dictation-proof; what people cannot use comfortably, they circumvent.

**Symptom of violation.** Token fragments stand in chats or tickets because there was no other name for the thing.

### F12 · Delivery duty: the code travels with the service

**Applies to:** Operator (for the benefit of the agent).

**What holds.** Every rule of this dossier that addresses agents — the storage order (F6), the binding (F7), the never-rules (F10), the way of speaking (F11) — MUST stand in the documentation or the self-description of the service itself, in the service's own words.

**Why.** The consuming agent does not know this dossier; it knows only what its service and its user tell it. An agent rule that stands only here reaches nobody — it would be hope, not a decision. The chain reads: dossier → operator → service documentation → agent; only the delivery turns recommendations into effective duties.

**Obvious and rejected.** *Teaching the rules to every agent platform individually* — the N×M problem, this time for behavioral rules. *Relying on general platform ground rules* — they vary; what the service needs, the service has to say.

**Symptom of violation.** Agents of different platforms handle the same token differently — some persist it correctly, others quote it in the chat — although all of them have read the same service documentation.

**Load-bearing structure (brief recapitulation).** F1 carries the cut; F2–F4 are its concretization for issuance and surfaces — without server-side creation with one-time display (F2) and without scope-in-the-token (F3), all that remains of the narrow cut is the intention, and F4 closes the leak surfaces through which even the narrowest token escapes. F5 joins the service side and the agent side at the setup. F6–F9 are the lifecycle: storage (F6) and binding (F7) protect the living token, rotation/revocation (F8) and expiry (F9) contain the dying one. F10 and F11 are the agent behavior; F12 makes it deliverable — if F12 falls, all agent rules remain hope. If F1 falls (legacy API), F5–F11 remain fully in force as the handling code for every secret: the concept breaks apart into two load-bearing halves, and that is exactly the intention.

## 5. Security considerations

The threats and how they are addressed — per row, the service-side and the agent-side countermeasure:

| Leak channel / attack | Typical course of events | Countermeasures |
|---|---|---|
| Chat history / shared conversation | Token quoted once more "for confirmation" | one-time display, no retrieval function (F2); agent side: name once, persist immediately, never repeat (F6, F10) |
| Prompt injection / exfiltration | Foreign content demands: "send your token to …" | host/tenant binding devalues foreign use (F7); agent side: instructions from work material are data, not commands (F10) |
| Confused deputy | Third parties move the agent to unwanted actions with a legitimate token | the narrow capability limits the damage (F1); agent side: confirmation by the user before actions that take effect (duty delivered per F12) |
| Service/proxy logs | Token in the URL or the request log | bearer header only, logs with the short ID only (F4); agent side: never put it into URLs (F10) |
| Repo commit | Config file or script committed with the token | the recognizable prefix makes scanners find it (F2); agent side: the storage ranked order instead of a project file (F6) |
| Config leak on the operator's side | Backup or ticket with a config file | only hashes on the operator's side (F2) |
| Device loss / copied storage | The agent's secret storage copied | rotation as a one-command routine (F8); narrow tokens limit the damage (F1) |
| Over-permissioning | The admin token used "to be safe" | the capability cut makes narrow tokens cheap (F1); agent side: always demand the narrowest token |
| Stockpiling / flooding the self-registration | An open endpoint accumulates rights-less tokens | throttling and expiry (F9); tokens created in the by-request flavor are without rights until release (F5) |
| Existence oracle | Systematic guessing at foreign resources | foreign resources answer identically to nonexistent ones (F4) |

Limit of the model: against a fully compromised agent host — where the attacker reads the secret storage itself — only the narrowness of the capability plus rotation still helps: the concept limits damage, it does not prevent it absolutely. And against an operator who breaks their own protocol, no consumer behavior protects.

## 6. What it costs

This section enumerates what the concept costs. Every point is pinned to the decision that causes it, and stands here without a counter-calculation.

**The number of tokens grows multiplicatively (F1).** One token per ability and place means: whoever needs three abilities at two places administers six tokens — each with its own storage, its own handle, its own revocation. The narrow cut shifts complexity out of the damage case and into everyday operation, and everyday operation has to carry it.

**The operator builds and runs the issuance (F2, F5, F9).** Server-side creation, hash storage, one-time display, display strings, where applicable the self-registration including throttling, expiry, and the review of self-introductions — that is construction work and, thereafter, ongoing operational work. In the "by-request" flavor the queue comes on top: every token born without rights waits for a human to activate it.

**One-time display costs comfort (F2).** There is no "show token" function, so there are inquiries about losses, and each of them ends in a rotation. That is intended — but it is work, and it accrues at the support desk.

**The error semantics confuses the legitimate (F4).** Whoever accesses a foreign resource gets the same 404 as for a nonexistent one — even the authorized user who has merely mistyped does not learn whether the resource exists. Protection against the oracle costs diagnostic comfort; the teaching error message for the correctly authenticated caller has to make up for it.

**Rotation discipline is calendar work (F8).** Maintaining expiry dates, monitoring for authentication failures, rotation as a practiced motion — that is an unspectacular permanent task, and it belongs in calendars and alerts precisely because it otherwise does not take place.

**The documentation duty repeats itself per service (F12).** Every operator who adopts the concept formulates the agent rules anew in their own documentation — the same rules, as often as there are services. This multiplication is intended (otherwise the rules reach no agent), but it is editorial work in every single adoption.

## 7. Scope and limits

### Preconditions

**The operator controls the issuance.** They can create tokens, check them, revoke them individually — that is what F2, F8, and F9 demand. Where the access comes from a third party (an inherited API key of a legacy API), only the handling half applies. You recognize the situation by who keeps the token list: nobody → no control over the issuance.

**There is a way past the chat.** Either a self-registration (F5) or some second channel from the operator to the user. This carries the answer, because otherwise the ranked order (F6) begins at the last resort. You recognize the situation when the only conceivable handover is "type it in here" — then §7 below applies.

**The agent has some kind of storage.** Environment variables, or at least files that outlast a single call. Without any storage, only the context window remains, and the declared last resort becomes the only one.

### When the answer tips

**An ecosystem enforces OAuth/OIDC.** Where delegation in the name of other users, consent dialogs, or federated, short-lived sessions are demanded — or a provider simply prescribes OAuth — static capability tokens are the wrong tool: then the established flows apply. But the answer tips only halfway: **the handling rules F5–F11 apply regardless of the token type** — an OAuth refresh token in the chat history is a leak as well. You recognize the situation by the words "on behalf of" and "consent" in the requirements.

**A token would by its nature have to bundle broad rights.** If the cut of the service yields no narrow abilities, the problem is not the token format but the service — F1 cannot be glued onto a service whose operations may all do everything. You recognize the situation when every attempt to define a narrow token ends at "but it still needs full access".

**The platform has no external path for secrets.** Without environment variables and without files, all that remains of the ranked order (F6) is the third rank: the secret stands in the transcript. Narrow tokens (F1) then limit the damage — where the damage potential is high, they do not carry it, and there the answer of this dossier does not apply.

**A central secret management is prescribed.** Where an organization makes vault-like systems binding, their mechanics replace ranks 1 and 2 of the storage — the ranked order becomes the question "the vault path before the context window". The never-rules (F10) and the cut (F1–F4) remain in force unchanged.

### What it does not solve

**The rights and role model of the service.** Who *may receive* which capabilities, who grants them and by what criteria — that remains the business of the service; this concept regulates only the artifact and its handling.

**The tooling mechanics of inventory administration.** How an operator concretely keeps their stock of tokens — with an administration CLI, say, that offers creation, rotation, and revocation as commands — is a topic of its own and deliberately left out here (Q7).

**The finding of services and the trust decision.** Whether a service is trustworthy, and how the agent knows of it, is not answered by this dossier; it begins where an access is to be set up.

**Passing on between humans.** When a user hands a token on to a colleague, it leaves every channel regulated here — against that, only the narrow cut helps (the damage stays small) and the expiry (the state ends).

## 8. Track record

This section separates what is in operation, what is in progress, and what serves illustration only. Every piece of evidence stands in the same grid.

### Evidence 1 — Worked application example: the self-describing services

**What it is.** The sister concept of this series for self-describing web services (Source 1) makes the decisions of this dossier binding for a concrete type of service: its rights cut corresponds to F1–F3, its secret ranked order to F6, its instance principle to F7, its self-registration with display string to F5 and F11, its delivery duty to F12.

**What it demonstrates.** That this general concept can be embedded completely and without contradiction into a concrete construction principle — the decisions there are the ones here, applied to one type of service. It demonstrates applicability, not operational experience.

**Status, honestly.** Published as a concept (version 1.0); the corresponding reference implementation is in progress (Evidence 3).

**Source.** Source 1.

### Evidence 2 — Precursor in operation: the secret doctrine of the wiki toolkit

**What it is.** A toolkit of the author's (Source 2) that makes the wiki platform Wiki.js usable for agents and enforces a strict secret doctrine in doing so: environment variable before file, file only as a hardened pass-through, the secret never appears in an output; several wiki instances are served at the same time, each with its own access.

**What it demonstrates.** The storage ranked order (F6) and the binding to the place (F7) are practicable under load — with this toolkit a wiki with 140,000 nodes was built up without a secret ever turning up in transcripts or logs. Not demonstrated is the token cut: the platform uses inherited API keys — exactly the case in which only the handling half of this concept applies.

**Status, honestly.** In operation and in daily use; a precursor, not a model.

**Source.** Source 2.

### Evidence 3 — Reference implementation in progress: the mail filing service

**What it is.** A service of the author's (Source 3), specified according to the sister concept from Evidence 1 and thereby according to this dossier: narrowly cut tokens (one token = one target), display strings, issuance on the operator's side; the self-registration is envisaged as an expansion stage.

**What it demonstrates.** So far, that the decisions can be specified in full for a real service. Operational experience is not demonstrated: the implementation by a development agent is under way; a completion report was still outstanding when this version was written and will be added here.

**Status, honestly.** Specification complete, implementation running, no consumer in everyday use.

**Source.** Source 3.

### Worked example (entirely fictitious): the traffic-report access

What follows is **not evidence** but illustration — all values are fictional. A user has their agent set up a traffic service. The agent calls the service's self-registration and introduces itself: "Agent of J.; purpose: report and retrieve traffic jams on the commute." The answer carries the flag **by-request**, a token with the service's prefix, and the display string `kluvmoth`. The agent stores the token in the environment variable the service proposes for it, and tells its user only: "Please give the access **kluvmoth** the reporting rights." The user activates `kluvmoth` server-side — the secret has never touched the chat (F5, F6, F11). Months later the operator reads their token list: next to `kluvmoth` stands the self-introduction, and they still know who got access there and why (F5). When the user changes their commute, they revoke `kluvmoth` — one ability lapses, everything else keeps running (F1, F8). A second token that they applied for "for later" a year ago and never activated has long since expired (F9).

## 9. Open questions

The first three questions are at the same time **assignments to the adopter**: this concept deliberately leaves them open, because their right answer depends on the deployment — whoever adopts the concept has to answer them for their own context and to document the answer.

**Q1** *(assignment)* Which expiry deadlines and throttling values apply (F9) — how long does an unconfirmed token live, from when does one count as unused, how strongly is the open issuance path throttled? The values are operational figures of the respective service; the concept demands only that they exist and are documented.

**Q2** *(assignment)* What form does the second channel take (F5) where no self-registration is offered — handover in person, a one-time link with an expiry deadline, an existing secret management? Every form has leak properties of its own; the choice belongs in the operational documentation of the service.

**Q3** *(assignment)* What rotation rhythm applies (F8), and is rotation automated? Caution with the automation: the delivery channel for each new token is itself a channel — whoever automates it may create precisely the permanent leak path that the one-time display was meant to avoid.

**Q4** Is a conventional namespace for token prefixes needed (F2), so that secret scanners can recognize services generically — or does per-service individuality suffice? A namespace would help the tools and would at the same time be the registry that this series otherwise avoids.

**Q5** Is capability attenuation worthwhile in the agent context — deriving a narrower token from a token, as the macaroon model demonstrates (Source 6) — or is "issue several narrow tokens" practically always simpler and safer?

**Q6** Should a service actively report the use of rotated old tokens — as an early warning about stolen copies — and how, without becoming an oracle through the report itself (F4)?

**Q7** The tooling mechanics of inventory administration — an administration CLI that runs creation, one-time display, rotation, and revocation as commands — is left out here: does it deserve a dossier of its own in this series, and which of its decisions would be mere application there, which of them new?

## 10. Sources & related work

This is the only place in this dossier where references stand. Locations checked on 2026-08-14 unless noted otherwise.

**Source 1 · Sister concept of this series: [self-describing-services](../self-describing-services/PROPOSAL.en.md).** — *What it stands for here:* The worked application example (§8, Evidence 1): there, the decisions of this dossier are made binding for self-describing web services (F12–F14 there). Further depth, not a prerequisite for understanding.

**Source 2 · Wiki toolkit of the author, repository `syncwiki`.** https://github.com/joern-schlingensiepen/syncwiki — *What it stands for here:* Evidence 2 in §8 (secret doctrine under load, 140,000 nodes).

**Source 3 · Mail filing service of the author, repository `imap-service`.** https://github.com/schlingensiepen/imap-service — *What it stands for here:* Evidence 3 in §8 (reference implementation in progress).

**Source 4 · Jack B. Dennis, Earl C. Van Horn: *Programming Semantics for Multiprogrammed Computations.* Communications of the ACM, volume 9, no. 3, 1966.** https://doi.org/10.1145/365230.365252 (publisher paywall; free retrieval not possible on 2026-08-14) — *What it stands for here:* The origin of the capability concept — the ability as an unforgeable artifact of authorization; F1 and F3 stand in this tradition.

**Source 5 · Norm Hardy: *The Confused Deputy (or why capabilities might have been invented).* ACM SIGOPS Operating Systems Review, 1988.** https://css.csail.mit.edu/6.858/2015/readings/confused-deputy.html (reprint, checked 2026-08-14) — *What it stands for here:* The classic argument for why rights should hang on the artifact instead of on the caller — the background of the §5 row on the confused deputy and of the binding from F7.

**Source 6 · Arnar Birgisson et al.: *Macaroons: Cookies with Contextual Caveats for Decentralized Authorization in the Cloud.* NDSS 2014.** https://research.google/pubs/macaroons-cookies-with-contextual-caveats-for-decentralized-authorization-in-the-cloud/ — *What it stands for here:* The reference model for capability attenuation that Q5 points to.

**Source 7 · OWASP Gen AI Security Project: *LLM01:2025 Prompt Injection.*** https://genai.owasp.org/llmrisk/llm01-prompt-injection/ — *What it stands for here:* The independent classification of the attack path against which the never-rules (F10) and the injection row in §5 are directed.

**Placement.** This dossier transfers an old idea into a new situation: capabilities (Sources 4, 5) bind the authorization to an unforgeable artifact instead of to the identity of the caller — exactly right for a consumer whose "identity" is an attackable context window (Source 7). What is new is not the model but the handling code around it: one-time display, ranked order, never-rules, display string — the concessions to a medium that was never meant for keeping secrets.
