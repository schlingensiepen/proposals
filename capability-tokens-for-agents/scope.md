---
type: Proposal Scope
description: Security considerations per leak channel, what the concept costs, where it applies, when the answer flips, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

The threats and how they are addressed — per row, the service-side and the agent-side countermeasure:

| Leak channel / attack | Typical course of events | Countermeasures |
|---|---|---|
| Chat history / shared conversation | Token quoted once more "for confirmation" | one-time display, no retrieval function (F2); agent side: name once, persist immediately, never repeat (F6, F10) |
| Prompt injection / exfiltration | Foreign content demands: "send your token to …" | host/tenant binding devalues foreign use (F7); agent side: instructions from work material are data, not commands (F10) |
| Confused deputy | Third parties move the agent to unwanted actions with a legitimate token | the narrow capability limits the damage (F1); agent side: confirmation by the user before actions that take effect (duty delivered per F12) |
| Service/proxy logs | Token in the URL or the request log | bearer header only, logs with the short ID only (F4); agent side: never put it into URLs (F10) |
| Repo commit | Config file or script committed with the token | the recognizable prefix makes scanners find it (F2); agent side: the storage ranking instead of a project file (F6) |
| Config leak on the operator's side | Backup or ticket with a config file | only hashes on the operator's side (F2) |
| Device loss / copied storage | The agent's secret storage copied | rotation as a one-command routine (F8); narrow tokens limit the damage (F1) |
| Over-permissioning | The admin token used "to be safe" | the capability cut makes narrow tokens cheap (F1); agent side: always demand the narrowest token |
| Stockpiling / flooding the self-registration | An open endpoint accumulates rights-less tokens | throttling and expiry (F9); tokens created in the by-request flavor are without rights until release (F5) |
| Existence oracle | Systematic guessing at foreign resources | foreign resources answer identically to nonexistent ones (F4) |

Limit of the model: against a fully compromised agent host — where the attacker reads the secret storage itself — only the narrowness of the capability plus rotation still helps: the concept limits damage, it does not prevent it absolutely. And against an operator who breaks their own protocol, no consumer behavior protects.

## What it costs

This section enumerates what the concept costs. Every point is pinned to the rule that causes it, and stands here without a counter-calculation.

**The number of tokens grows multiplicatively (F1).** One token per ability and place means: whoever needs three abilities at two places administers six tokens — each with its own storage, its own handle, its own revocation. The narrow cut shifts complexity out of the damage case and into everyday operation, and everyday operation has to carry it.

**The operator builds and runs the issuance (F2, F5, F9).** Server-side creation, hash storage, one-time display, display strings, where applicable the self-registration including throttling, expiry, and the review of self-introductions — that is construction work and, thereafter, ongoing operational work. In the "by-request" flavor the queue comes on top: every token born without rights waits for a human to activate it.

**One-time display costs comfort (F2).** There is no "show token" function, so there are inquiries about losses, and each of them ends in a rotation. That is intended — but it is work, and it accrues at the support desk.

**The error semantics confuses the legitimate (F4).** Whoever accesses a foreign resource gets the same 404 as for a nonexistent one — even the authorized user who has merely mistyped does not learn whether the resource exists. Protection against the oracle costs diagnostic comfort; the teaching error message for the correctly authenticated caller has to make up for it.

**Rotation discipline is calendar work (F8).** Maintaining expiry dates, monitoring for authentication failures, rotation as a practiced motion — that is an unspectacular permanent task, and it belongs in calendars and alerts precisely because it otherwise does not take place.

**The documentation duty repeats itself per service (F12).** Every operator who adopts the concept formulates the agent rules anew in their own documentation — the same rules, as often as there are services. This multiplication is intended (otherwise the rules reach no agent), but it is editorial work in every single adoption.

## Where it applies

### Preconditions

**The operator controls the issuance.** They can create tokens, check them, revoke them individually — that is what F2, F8, and F9 demand. Where the access comes from a third party (an inherited API key of a legacy API), only the handling half applies. You recognize the situation by who keeps the token list: nobody means no control over the issuance.

**There is a way past the chat.** Either a self-registration (F5) or some second channel from the operator to the user. This carries the answer, because otherwise the ranking (F6) begins at the last resort. You recognize the situation when the only conceivable handover is "type it in here" — then the conditions below under which the answer flips apply.

**The agent has some kind of storage.** Environment variables, or at least files that outlast a single call. Without any storage, only the context window remains, and the declared last resort becomes the only one.

### When the answer flips

**An ecosystem enforces OAuth/OIDC.** Where delegation in the name of other users, consent dialogs, or federated, short-lived sessions are demanded — or a provider simply prescribes OAuth — static capability tokens are the wrong tool: then the established flows apply. But the answer flips only halfway: **the handling rules F5 to F11 apply regardless of the token type** — an OAuth refresh token in the chat history is a leak as well. You recognize the situation by the words "on behalf of" and "consent" in the requirements.

**A token would by its nature have to bundle broad rights.** If the cut of the service yields no narrow abilities, the problem is not the token format but the service — F1 cannot be glued onto a service whose operations may all do everything. You recognize the situation when every attempt to define a narrow token ends at "but it still needs full access".

**The platform has no external path for secrets.** Without environment variables and without files, all that remains of the ranking (F6) is the last rank: the secret stands in the transcript. Narrow tokens (F1) then limit the damage — where the damage potential is high, they do not carry it, and there the answer of this concept does not apply.

**A central secret management is prescribed.** Where an organization makes vault-like systems binding, their mechanics take the place of the environment and the pass-through file, or feed them; whether as a rank of its own or as a feeder is the further-sources choice of `where-a-secret-comes-from`. The ranking then becomes the question "the vault path before the context window". The never-rules (F10) and the cut (F1 to F4) remain in force unchanged.

### What it does not solve

**The rights and role model of the service.** Who *may receive* which capabilities, who grants them and by what criteria — that remains the business of the service; this concept regulates only the artifact and its handling.

**The tooling mechanics of inventory administration.** How an operator concretely keeps their stock of tokens — with an administration CLI, say, that offers creation, rotation, and revocation as commands — is a topic of its own and deliberately left out here.

**The finding of services and the trust decision.** Whether a service is trustworthy, and how the agent knows of it, is not answered by this concept; it begins where an access is to be set up.

**Passing on between humans.** When a user hands a token on to a colleague, it leaves every channel regulated here — against that, only the narrow cut helps (the damage stays small) and the expiry (the state ends).

### Not yet settled

* Whether a conventional namespace for token prefixes is needed (F2), so that secret scanners can recognize services generically, or whether per-service individuality suffices. A namespace would help the tools and would at the same time be the registry that this series otherwise avoids.
* Whether capability attenuation is worthwhile in the agent context — deriving a narrower token from a token, as the macaroon model demonstrates (see the sources) — or whether "issue several narrow tokens" is practically always simpler and safer.
* Whether a service should actively report the use of rotated old tokens, as an early warning about stolen copies, and how it can do so without becoming an oracle through the report itself (F4).
