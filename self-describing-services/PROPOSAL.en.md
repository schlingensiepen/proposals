---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: self-describing-services
language: en — translated from the German original
sync: v1.0 (2026-08-14)
question: "How, as a user, do I make a service available to my agents — without installation, on every platform, in every sandbox?"
thesis: "A service that describes itself makes installation unnecessary: the agent remembers only the classic tool description — if you want this, look at this URL — and a token; everything else the service delivers itself at runtime, and because there is no second source, nothing goes stale."
---

# Self-Describing Services

**In one sentence:** A service that describes itself makes installation unnecessary — the agent remembers only the classic tool description ("if you want this, look at this URL") and a token; everything else the service delivers itself at runtime, and because there is no second source, nothing goes stale.

**Abstract:** Giving an agent a service is an installation project today: install a connector, distribute a skill, click through permissions — and every installed description begins to go stale immediately. A self-describing service collapses onboarding into one sentence to remember — "if you want this, look at this URL": the classic tool description, a purpose and an address. Everything else — entry page, usage description, interface description — the agent fetches from the service itself, up to date; the staggered delivery exists precisely so that this one sentence suffices. It works in every sandbox, because it demands only two capabilities that every agent has: HTTP and remembering.

## 1. Problem

The author asked himself this question as a **user**: there is a service — a mailbox filing service, a wiki, a domain API — and there are agents on various platforms, in cloud sandboxes, in the terminal, in the browser. Why is "please use this" an installation project? For every platform a connector, for every connector an installation, for every installation permission dialogs — and in the end the agent still knows only what somebody wrote down for it weeks ago.

Four roles carry the problem together:

**The user** wants to be able to say "add this service" — as a sentence, not as a project. He has several agents on several platforms, and many of them run in sandboxes where simply nothing can be installed. What an agent can do there is: speak HTTP and remember something. An onboarding that demands more excludes precisely those environments in which agents increasingly work.

**The operator** of a small or internal service cannot build, publish, and keep current a connector for every agent platform. Every distributed description of his service — skill file, connector, wiki page — is a copy he will never fully round up again: this is an N-services-times-M-platforms problem, and it grows with every release.

**The retrofitter** faces a foreign existing API that he neither owns nor can change — a wiki, a ticket system, a domain API — and wants to make it usable for his agents. All that remains to him today is the distributed skill file: a copy of someone else's knowledge that grows quietly wrong with every update of the target service and that he has to deliver to each consumer individually.

**The agent**, finally, believes its instructions literally. A human developer grows suspicious when the documentation does not match the API, and starts experimenting; an agent produces perfectly formatted, wrong calls from an outdated description — with confidence. For him, instructions that *can* be outdated are instructions that will *lie* at some point.

The usual way out — more discipline in maintaining the documentation — fights the symptom. The structural answer starts elsewhere: installation and copying are the problem. What is not distributed does not have to be installed; what does not exist separately cannot go stale separately.

## 2. The concept

### 2.1 The starting point: interfaces almost describe themselves already

A web interface starts out doing something very ordinary: it offers operations — read data, write data, accept files — and precisely this **mechanics** is today largely documented automatically. Modern frameworks generate the interface description directly from the code: with HTTP APIs, **OpenAPI** is the established form — routes, fields, types, status codes fall out of the build without being maintained, and they sit machine-readable at a URL of the service itself. That is an excellent starting point: the contract already exists, it is already generated, it is already retrievable. All that is left is to **adapt it to be agent-friendly** — to "pump it up": every field described, every example real, every error explained — and the formality turns into a function.

### 2.2 The parallel: what MCP is for tools, the web API becomes for services

The Model Context Protocol has demonstrated what agent integration without knowledge distribution looks like: an MCP server describes its many methods **at runtime**, the client queries them — discovery instead of distribution. This concept carries exactly that principle over to the web API. The decisive difference: an MCP server first has to be *connected* at the consumer's end — an entry, a process, a permission. The web client, by contrast, every agent has built in long since; HTTP is the one capability that is present everywhere, right into the locked-down sandbox. So all we are doing is moving the self-description of an MCP onto the interface that needs no connecting.

### 2.3 Two kinds of description — and a deliberate breach

Knowledge about a service falls into two kinds. The **interface description** carries the mechanics: what exists and how to call it correctly. The **usage description** carries the overarching behavior: what the service is meant for, in what order one works, which duties apply toward the user, how access is to be handled. But the clean separation is only the beginning — we **breach it on purpose**: when pumping up, every piece of behavior that clings to a single operation ("this call is asynchronous", "this needs your user's confirmation", "this error means: upload first") migrates *into the interface description*, right next to the operation. The gain is twofold: the agent finds the rule exactly where he builds the call — and the usage description stays small, because it now carries only what is genuinely overarching.

### 2.4 The entry page: hypertext, rendered at runtime

Everything begins at a single address: the **entry page** at the base URL is an unauthenticated short description of some 20 lines — *and* the directory of everything else. It says what the service is and how to obtain access, and it **links every part of the description**: the usage description, the interface description, the thematic part descriptions (each with one sentence on what the part does), the anchor skill. This is the old strength of hypertext, applied to self-disclosure: you need to know nothing but one address — from there everything is reachable, and at every link it says what awaits you behind it. Whoever builds such an entry page inserts the URLs **at runtime**, with the host under which the service is currently being addressed — which makes every example copyable for exactly this installation. The sequence "short description – link – long description – link – …" is not a stylistic device but **context-window thrift**: at each step only what the next decision needs is loaded — the rest stays a link. And hypertext is meant here as a **concept**, not as a format: the steps are as a rule Markdown with links — immediately readable for agents and humans alike; HTML is not needed for it.

### 2.5 A walkthrough: the traffic service

What this feels like is shown by an invented service that accompanies us through the whole dossier *(entirely fictitious and cut for teaching purposes; the real track record is in §8)*: a service that **collects and provides traffic data**. Its entry page divides the interface into four explained parts: **learning the traffic situation** (the frequent read case) · **contributing reports** (the write case) · **uploading data sets** in various formats (rarely needed, extensive) · **filing and retrieving POIs** (static points of interest — useful, but not in every use case). An agent never comes "just because"; he comes with a concern: he wants to *learn* something or to *contribute* something. At the entry page he takes the branch and loads **only** the part description for his concern — the upload formats and the POI administration he does not even read, and he does not clog his context window with them. But because he knows the short version, he knows forever *where* he has to look if his user does want to maintain POIs next week.

And the walkthrough shows one more thing: **a service can occur several times in a user's life.** The traffic service of one's own city and that of the holiday destination, the company wiki and the private one — same software, same API, but each with its own base URL, its own rights, its own secret. What is remembered is therefore always the **instance**, never "the software"; what that means in detail is governed by F9.

### 2.6 Building instructions — and retrofitting along with them

The decisions in §4 are **building instructions**: they say how one *writes* such a self-describing interface. And they apply twice over from the very start. Either you build a **service of your own** and deliver the description out of it — or you **retrofit a foreign existing API**: a small description service of your own, the **façade**, delivers the complete cascade for the foreign API and tells the agent "execute the operations described here directly against the original service". The façade touches no data traffic; it makes an API that does not describe itself *immediately usable*. Building and retrofitting are the same instructions with a different place of delivery — which is why the façade appears here at the beginning as an application and at the end (F15) as a decision.

### 2.7 Access: in every case a secret — sometimes it is a token

That leaves access. For a **new build** we recommend the token model: narrowly cut tokens, each embodying exactly one capability (F12). With an **existing API**, the authentication brings along whatever it has — an API key, a password. Either way: **in every case you have a secret; sometimes it is a token.** And for every secret, whatever its origin, the same handling order of precedence applies — environment variable before pass-through file before context window; the details are governed by F13.

### 2.8 The chain of transmission: who reads this dossier — and who does not

A consuming agent does not know this dossier, and he does not need to: he knows only the service's self-description, and that is self-contained. Therefore every rule of this dossier that addresses consumers is in truth a **delivery duty of the builder**: it must appear in the service's own usage description, in the service's own words. The chain reads *dossier → builder → self-description → agent* — this document instructs the one who builds; the service instructs the one who uses (F11).

## 3. Terms

**Interface description** — the machine-readable description of the mechanics: operations, fields, types, error formats, examples; generated from the code and editorially enriched (with HTTP APIs typically OpenAPI).

**Usage description** — the prose document delivered by the service, carrying the overarching behavior: what the service is meant for, workflow, duties toward the user, handling of the secret.

**Entry page** — the unauthenticated short description (~20 lines) at the base URL; it lists and links all remaining parts of the description together with their purpose.

**Cascade** — the staggering of entry page → usage description → interface description (including the part descriptions), ordered by loading cost: decide first, then load.

**Part description** — the thematic excerpt of the description for one delimited area of tasks (in the traffic service: learn · contribute · data-set upload · POI); loadable on its own, listed with its purpose on the entry page.

**Anchor skill** — the storable skill provided by the service, containing nothing but purpose, target, and links to the self-description: an empty shell that anchors triggering in the user's skill catalog. *Not to be confused with the façade:* the anchor skill forwards to the service, the façade describes a foreign API.

**Façade** — a description service of one's own that delivers the cascade for a foreign existing API without touching its data traffic (retrofit).

**Tenant** — a delimited area of use within a multi-tenant service (its own identifiers, its own rights, possibly its own hostname); discovery decisions can be taken per tenant.

**Instance** — a concrete installation of a service, or of a piece of service software, under its own base URL. The same software can occur several times in a user's life (the company wiki, the private wiki); remembering, anchoring, and granting rights happen per instance.

**Harness** — the runtime environment in which an agent runs, together with its storage options: session memory, skill catalog, environment variables. The harness belongs to the user, or to his platform, not to the service.

**Secret** — every confidential credential the consumer needs in order to make a call: API key, password — or a token.

**Capability** — a single, narrowly delimited ability at a service (for instance: contributing reports). The cut of rights in F12 thinks in capabilities; the token is the artifact that embodies exactly one of them.

**Token** — the recommended access artifact for new builds: narrowly cut, one capability per token. *Every token is a secret; not every secret is a token.*

**Self-registration** — the optional open method by which a consumer receives a token in exchange for a free-text self-introduction; in two flavors: automatic and by-request (→ F14).

**Display string** — the short, pronounceable, **non-secret** handle to a token (8 characters, lowercase) by which humans talk about tokens and grant rights without ever naming the secret (→ F14).

## 4. Decisions

**Map.** Fifteen decisions in four blocks. **F1–F7** address the *builder* — they say how the description comes into being and how it is delivered; in a retrofit they apply to the façade operator. **F8–F11** govern the *consumer side*: how a service is offered, remembered per instance, and made available — with F11 as the bridge: consumer rules reach the agent exclusively via the self-description that the builder delivers (§2.8). **F12–F14** are the access gear — token recommendation, secret handling, self-registration — and they carry independently: they hold even where nothing else of this concept is adopted. **F15** extends the whole thing to foreign services. F1 carries everything: if "one source" falls, the rest decays into ordinary documentation advice. Every decision names whom it applies to; the worked example for the whole flow is in §8.

### F1 · One source: the service describes itself

**Applies to:** builder (in a retrofit: façade operator).

**What holds.** The authoritative description of a service — entry page, usage description, interface description — is delivered by the service itself at runtime; a separately distributed and separately maintained description does not exist.

**Why.** Every copy begins to go stale the moment it is created, and agents believe instructions literally: an outdated description gives rise to perfectly formatted wrong calls. At the same time, with distribution the installation falls away too — and with it the N×M problem of services and platforms.

**Obvious and rejected.** *Distributing skill files* — they go stale and have to be brought to every consumer; precisely the original problem. *A connector or broker service per platform* — a second implementation of the API that has to be installed and that drifts on its own. *Documentation discipline and release checklists* — they fight the symptom; structurally the second source remains.

**Symptom of violation.** Two descriptions of the same service contradict each other in a detail. The early indicator is small: an operation count that reads "12" in one artifact and "13" in the neighboring document — a case actually observed, resolved in §8.

### F2 · Entry page and cascade: everything linked, decide first, load second

**Applies to:** builder.

**What holds.** The unauthenticated entry page (~20 lines at the base URL) suffices for the decision whether the service is needed — and it **lists and links every part of the description**: usage description, interface description, every part description (each with one sentence of purpose), and the anchor skill. The steps are linked to one another; only what is actually needed is loaded on, and only once the decision has been taken — all description steps are reachable without authorization (F4).

**Why.** The context window is the consumer's scarce good: an agent must be able to reject a service *before* he has read it in full. And the full linking is the hypertext strength that makes remembering minimal — whoever knows one address reaches everything from there and knows at every link what awaits him behind it.

**Obvious and rejected.** *A single large description document* — forces a full-text load before the decision. *Fixed, standardized paths as a precondition* — recognizability is pleasant, but the entry page opens up the rest by reference; a path convention is a comfort, not a condition (deliberately left open: Q1).

**Symptom of violation.** Agents regularly load the complete description and only afterwards discover that the service does not fit — or they find a part of the description only because somebody outside the service has linked to it.

### F3 · Thematic division with an explained structure

**Applies to:** builder.

**What holds.** The description SHOULD be broken down into thematic parts whose structure the entry page itself explains: at every link to a part it says what the part does. The builder chooses the axes according to the consumers' concerns — for instance core function vs. administration, data access vs. rights assignment, file handling as a group of its own, because not every use case involves files. The same division applies to the anchor skill package: one file per topic.

**Why.** An agent comes with a concern, not with an appetite for reading: at the traffic service he loads "contribute" and leaves upload formats and POI administration unread — the context window stays free, and because the short version explains the structure, he knows permanently where the unread material lives. An unexplained structure, by contrast, forces him to grope around: then he loads everything after all.

**Obvious and rejected.** *Explaining everything in one piece* — punishes every consumer with the full load, even for the smallest question. *Division by internal modules instead of by concerns* — the consumer thinks in "I want to learn/contribute something", not in the operator's module names.

**Symptom of violation.** For a simple query, agents regularly load the full description — or the consumers' questions spread crosswise to the parts on offer, because the axes miss their concerns.

### F4 · Discovery without authentication, without secrets

**Applies to:** builder.

**What holds.** Entry page, usage description, and interface description are retrievable without credentials and contain neither secrets nor operational internals — and this via a simple **GET**, detached from whichever dialect the API itself speaks: even a service that offers GraphQL or JSON-RPC delivers the description as plain, linkable GET resources. Where the open disclosure is a nuisance, it can be switched off per tenant — as a deliberate trade-off, not as a default. A builder who nevertheless wants a bolt on the door MAY put minimal protection in front of it: an obscurity token in the entry page's URL, which on rendering (F5) is encoded into all onward links. This is deliberately weak protection — URL components end up in logs and histories, and such a token is not a secret in the sense of F13 — and the recommendation remains: in the simplest case, completely open.

**Why.** Discovery has to work *before* the consumer has credentials — otherwise nobody can find out how to obtain any. And what is retrievable by everyone may contain nothing that not everyone is allowed to know; this content rule is easier to check than any access rule.

**Obvious and rejected.** *The description behind a login* — chicken and egg: the way to access presupposes access. *"Internals do no harm, only the customer sees them anyway"* — open endpoints have no customer list.

**Symptom of violation.** Onboarding instructions circulate outside the service again — in wikis and chats — because the service itself does not hand them out; or the open description contains something an operator would rather delete on reading it.

### F5 · Live templating with the real base URL

**Applies to:** builder.

**What holds.** All examples in entry page, usage description, and interface description are rendered with the host under which the service is currently being addressed.

**Why.** Every example is thereby copy-paste ready for exactly this installation. Placeholders are a source of error precisely with the consumer who takes things literally.

**Obvious and rejected.** *`example.com` placeholders with a "replace with your host" footnote* — agents actually do call placeholders. *Rendering a configured "official" URL* — behind proxies and with tenants it does not match the request.

**Symptom of violation.** The logs contain requests against `example.com`.

### F6 · The pumped-up contract

**Applies to:** builder.

**What holds.** The generated interface description is treated as a product and editorially enriched — with OpenAPI concretely: **(1)** every field carries a description that does not repeat the name but names semantics, units, limits, and typical errors, plus a realistic example; **(2)** every endpoint carries a short and a long description with the non-obvious rules — this is where the behavior that clings to the operation migrates (§2.3); **(3)** the preamble contains the workflow in short form in a few lines; **(4)** errors are documented like successes, every error type with an example response; **(5)** all of this is enforced as a check ("no operation, no field without a description and an example" as a gate in the build). Other interface dialects bring their own contract mechanisms — GraphQL its introspection with description fields, JSON-RPC the OpenRPC document, gRPC its reflection — and are enriched analogously; appendices per technology are envisaged, in case it turns out that the analogy has to be spelled out in detail.

**Why.** The contract exists anyway (§2.1) — enriched, it turns from a formality into complete documentation of the mechanics that cannot go stale, because it falls out of the same code as the behavior of the service. And the curation *inside* the carrier is what distinguishes it from a curated side list: it is delivered along with the carrier and does not age separately.

**Obvious and rejected.** *Deliver the raw schema and be done with it* — completeness without weighting; the consumer does not learn what is important, what belongs together, what has which consequences. *A separate curated operation list next to the schema* — the author has built this precursor himself; it works and proves the value of curation, but it produces three artifacts aging side by side (§8). *Mandatory descriptions as an intention rather than as a gate* — erodes with the first hurried release.

**Symptom of violation.** The prose instructions grow while the interface's description fields stay empty — or a consumer needs a second document besides the contract in order to build a single call correctly.

### F7 · Teaching error messages

**Applies to:** builder.

**What holds.** Every rejection names the correct path — concretely, with the next step. At the traffic service, the upload endpoint answers an embedded image not with "invalid request" but with:

```json
{
  "error": "inline_image_not_accepted",
  "message": "Images cannot be embedded in a report. Upload the image first:
              POST https://verkehr.example.org/api/v1/files (returns a file_id),
              then reference it as \"photo\": {\"file_id\": \"...\"}."
}
```

**Why.** The moment of failure is the moment of highest attention: with the agent, the correction instruction is in the context window exactly when it is needed — self-description at the point of failure. Error texts are interface description and deserve the same editorial care. It is at the same time the redemption of an old idea: HATEOAS wanted responses that name the next steps themselves — it only came too early, because human developers did not want to discover anything at runtime; agents do it without complaint.

**Obvious and rejected.** *Generic error codes with a pointer to the documentation* — a break in the medium; the agent guesses instead of reading. *Error texts as an implementation detail* — then they explain the internal state instead of the way out.

**Symptom of violation.** An agent repeats the same failing call several times in a row — the response has taught him nothing.

### F8 · The anchor skill: a shell of links, actively offered

**Applies to:** builder.

**What holds.** The service provides a classic, storable skill that contains **no knowledge** but exclusively forwards: purpose of the service · target (base URL) · link to the self-description · one (purpose, link) pair per part description. The package is divided into files along the thematic division (F3). And the entry page **actively offers this skill** — a sentence of the form "You can save or install this service as a skill: <link>", together with the reference — because what is merely listed somewhere falls into oblivion.

**Why.** Persistence is useful for *triggering*, not for *knowing*: a stored skill triggers when the user says something pertinent — but only an empty shell cannot go stale; what can go stale is solely its references, when the parts are recut (§6 names the cost). The anchor skill anchors triggering in the usual agent infrastructure while itself remaining a forwarding layer made of links; resolution happens only at the time of use. The active offer on the entry page makes the proven flow reliable: "install `<url>` for me" suffices, and the agent finds the skill because the entry page holds it out to him.

**Obvious and rejected.** *Storing complete copies of the instructions in the skill* — drift; precisely the second source that F1 forbids. *Offering nothing storable at all* — then nothing triggers, and the user has to name the URL every time. *Calling the thing a "façade"* — the term is taken by the retrofit (F15); one word with two meanings would be exactly the confusion the terms section is meant to prevent.

**Symptom of violation.** A locally stored skill describes yesterday's interface — or users use the service for months without knowing that the skill exists.

### F9 · One instance, one anchor

**Applies to:** consumer (delivered by the builder, F11).

**What holds.** Remembering, anchoring, and authorizing happen per **instance**, never "per software": every instance of the same service gets its own anchor with its own purpose, its own base URL, and its own secret. If the user has anchored several instances of the same service, the agent asks back when things are ambiguous ("put that in the wiki" — but which one?) instead of guessing.

**Why.** Instances share the API, but nothing else: different data, different rights, different secrets. Whoever conflates them writes into the wrong wiki or uses one instance's secret against the other — with the same, perfectly fitting call mechanics that will report no error.

**Obvious and rejected.** *One anchor per piece of service software with a list of instances inside it* — merely shifts the risk of confusion inward and lets the anchor grow into the registry that this concept deliberately does not build (F10). *Guessing the instance from context* — the agent who takes things literally also guesses literally wrong.

**Symptom of violation.** A contribution lands in the wrong instance — or a secret is used against a base URL other than the one it belongs to.

### F10 · Making available at scopes — with the harness's own means

**Applies to:** consumer (delivered by the builder, F11).

**What holds.** The user makes the service available with natural language — "make `<url>` available for this session", "install that permanently for me", "make that available for all users" — and for this the agent uses the storage mechanisms of his own harness: session memory, skill catalog, whatever is present. This concept deliberately does not standardize the mechanics.

**Why.** Every harness stores differently, and it knows best itself — a prescribed storage structure would be installation through the back door. The path is proven: on hearing the three scope statements, at least one widespread agent environment fetches and stores the anchor skill correctly in the respectively right place — and it is exactly for this that F8 laid down the active offer with a link on the entry page: the agent finds what the entry page holds out to him. Because cascade and division leave almost nothing in what is stored, storage stays cheap — resolution happens at the time of use.

**Obvious and rejected.** *A standardized registry of remembered services* — the agent has his own mechanisms; we would have invented a structure that every harness solves differently and better itself. *Persistence as a duty* — "for this session" is a legitimate end state; the scope is the user's business.

**Symptom of violation.** The agent demands format or storage knowledge from the user ("where should I write this?") — or the user has to name the URL anew every time, because nothing was anchored.

### F11 · Delivery duty: the code of conduct travels with the service

**Applies to:** builder (for the benefit of the consumer).

**What holds.** Every rule of this dossier that addresses consumers — the scope question to the user, an anchor per instance, asking back when things are ambiguous, the secret order of precedence, confirmation before actions that take effect, display string instead of token — MUST appear in the service's own usage description, in the service's own words.

**Why.** The consuming agent does not know this dossier; he knows only the self-contained self-description of the service (§2.8). A consumer rule that appears only here reaches nobody — it would be a hope, not a decision. The chain dossier → builder → self-description → agent turns behavioral recommendations into deliverable duties.

**Obvious and rejected.** *Teaching the rules to every consumer harness individually* — the N×M problem, this time for behavioral rules. *Relying on general agent ground rules* — those vary by platform; what the service needs, the service has to say.

**Symptom of violation.** Agents on different platforms behave differently at the same service — some ask before actions that take effect, others do not — although all of them have loaded the same description.

### F12 · Narrow tokens as the recommendation for new builds

**Applies to:** builder.

**What holds.** A newly built service SHOULD cut access rights as narrow capabilities: one token embodies exactly one capability at exactly one place; whoever needs two targets gets two tokens. **The base path of issuance is the operator:** he generates the token server-side and displays it exactly once. Deviations from the token model are permissible with good reasons — an existing API brings along whatever it has (§2.7).

**Why.** In the agent context, the chat history is a real leak channel. Narrow tokens make every single leak surgically small — revocation and rotation hit exactly one purpose, not the whole service. The deepened justification together with the handling code is carried by the sister dossier *capability-tokens-for-agents*; this decision summarizes what is needed here.

**Obvious and rejected.** *An admin token "just to be safe"* — makes every leak total. *A scope parameter on the call, checked against the token* — the token itself is meant to be the scope; a selectable parameter is authorization code that can be wrong.

**Symptom of violation.** Revoking a single token shuts down more than the one purpose it was issued for.

### F13 · Secret handling in a fixed order of precedence

**Applies to:** consumer (delivered by the builder, F11).

**What holds.** The usage description prescribes to the consumer where to obtain the secret, in this order of precedence: **1.** environment variable — the usage description MAY propose a canonical name for this (the traffic service, for instance, `TRAFFIC_API_TOKEN`); **2.** if the environment cannot do that: a file with narrow permissions, used exclusively as a **pass-through** — the content flows directly into the call and never appears in outputs, logs, or the context window; **3.** only as a last resort the context window itself — name it once, persist it immediately, never repeat it. The order of precedence applies to every secret, whether a token or an inherited API key. **How the secret reaches the consumer in the first place is deliberately left open by this concept** — often it takes a second channel from the operator to the user, outside the chat. For the simple case with low damage potential, we recommend starting with self-registration (F14) — in one of its two flavors: **automatic** or **by-request**.

**Why.** Chat histories and logs are searchable, copyable stores. The order of precedence minimizes how long and how often a secret exists in them — and it gives the agent a checkable rule instead of a feeling. The canonical variable name incidentally solves the problem of finding it again across sessions: the service itself says where its secret usually lives.

**Obvious and rejected.** *"Just put the secret in the chat" as the normal path* — turns the transcript store into a keyring. *Client-side encryption contortions* — the agent cannot keep secrets from himself; false security.

**Symptom of violation.** A secret turns up in a transcript, a log, or an error message.

### F14 · Optional self-registration with a display string

**Applies to:** builder (the offer) · consumer (the flow; delivered by the builder, F11).

**What holds.** A service MAY offer an open registration method alongside the open description methods. The flow in three steps: **(1)** The agent calls it with a free-text **self-introduction** (who, what for). **(2)** The service answers with three things: the **token**, a **display string** (8 characters, lowercase), and a **flag** naming the flavor — **automatic** (pure self-registration: the token is usable immediately) or **by-request** (the token first needs an activation). **(3)** In the "by-request" flavor, the agent shows his user **the display string, never the token** — and the user grants the rights server-side to exactly this display string. In this case the token comes into the world without rights and is switched live afterwards. Protecting the open endpoint against abuse (throttling, expiry of unconfirmed tokens) is the operator's task; the self-introduction is stored with the token and is the basis for deciding about it and for auditing it.

**Why.** Onboarding thereby becomes end-to-end — "URL plus self-introduction" — without a secret ever passing through humans: the display string is the pronounceable, non-secret handle to the token. Whoever reads the token list months later can still tell from the self-introduction who obtained access there and why. And let it be said honestly: **this is not really secure** — it is the deliberate compromise for getting started quickly. The author has tried out more complicated procedures; they made onboarding unusable. The limits are treated by the security considerations (§5).

**Obvious and rejected.** *A full OAuth build-out* — disproportionate for small services; where an ecosystem prescribes it, it applies anyway. *The token itself as the reference in conversation* — then the secret travels through chat and word of mouth. *Ordering the rights in the same call* — then the anonymous party decides on his own cut; separating creation from activation is the protection.

**Symptom of violation.** Users dictate tokens through chats or ticket systems — or onboarding fails because nobody with server access happens to be at hand.

### F15 · Retrofit: the façade describes, it does not mediate

**Applies to:** façade operator (retrofitter).

**What holds.** For an existing API, the cascade MAY be delivered by a description service of its own that does not touch the data traffic. In doing so the façade delivers the **complete self-description according to this building principle** — entry page, usage description, pumped-up interface description (F6), anchor skill — except that the description says explicitly: "execute the operations described here directly against `<original endpoint>`". In the live templating (F5), the **real target** is rendered for this, never the façade itself. The façade names the version, or the state, of the target API that it describes; the way to check against drift of the target API is deliberately left open (Q4). Whoever wants to serve several instances of the same service software is recommended the **generic** build form: a description service that is given the target instance in the URL — `https://wikijs-fassade.example/refer-to/wiki.anwender.org` describes the same software, rendered onto exactly this instance. The example is deliberately a wiki: precisely there one often has access to several instances that all follow the same API.

**Why.** This makes a foreign API immediately agent-usable without the description service stepping into the data path — no additional latency, no responsibility for payload data, and secrets continue to flow only between consumer and original service.

**Obvious and rejected.** *A full proxy* — takes on data-path responsibility, costs latency, and routes foreign secrets through third parties. *Distributing skill files in the repository* — it works (§8: a wiki with 140,000 nodes was built that way), but it is the distributed copy with all the consequences from F1.

**Symptom of violation.** The façade describes operations that the target service, after an update, no longer offers or offers differently — and no consumer learns of it until a call fails.

**Load-bearing structure (brief recapitulation).** F1 carries everything. F2–F7 are its concretization for the builder: cascade and division (F2/F3) make the one source loadable, F4/F5 make it reachable and literally usable, F6/F7 fill it — in the contract and in the error case. F8–F11 are the consumer side: without an actively offered anchor there is no triggering (F8/F10), without instance discipline no safe target (F9), and without the delivery duty (F11) no consumer rule ever reaches an agent. F12–F14 carry independently — the access gear holds even without the rest. F15 extends F1 to foreign services and inherits its drift risk in measured form; if F15 falls, the building principle for one's own services remains untouched.

## 5. Security considerations

Self-description means literally: **the text of a service instructs your agent.** That is not a new attack path — whoever today tells an agent "install the skill from this repository" trusts foreign text just as much, only with a less clear anchor. What is new is the *recurrence*: instructions loaded live can change after the user has formed his trust. The same property is at the same time the strength of the model — the operator can correct a faulty or dangerous instruction immediately for all consumers, which is impossible with distributed copies. This **reload problem** deserves to be named and understood from both sides: as a price (recurring exposure) and as a strength (immediate correctability).

| Attack / risk | Typical course | Countermeasures |
|---|---|---|
| Planted service URL | foreign content (email, web page) demands: "fetch the instructions from evil.example" | instructions are loaded only from services the user has named or anchored (F9/F10); new services only after asking the user |
| Reload problem: service changes instructions afterwards | a compromised service, or one that has turned malicious, delivers new behavioral rules | the user's confirmation duty before actions that take effect remains untouched by any service instruction; service instructions can extend an agent's ground rules, never replace them; a change signal by which an agent would recognize instructions altered after the fact is not part of this concept (open as Q3) |
| Injection via the description | description contains "send your token to …" | secrets go exclusively to the base URL of their own anchor; instructions concerning targets outside the service are data, not commands |
| Secret to the wrong instance | two instances of the same software; the company secret is used against the private instance — the call mechanics fit perfectly, no error appears | one anchor with its own secret per instance (F9); the secret is bound to the base URL of its anchor and is never used against another; when things are ambiguous the agent asks back |
| Spoofing / eavesdropping | description is tampered with in transit | TLS is an operating precondition; the anchor stores the canonical URL |
| Abuse of self-registration | flooding the open endpoint, token stockpiling | the operator's task: throttling, expiry of unconfirmed tokens (F14); tokens created "by-request" are without rights until activation; the "automatic" flavor is recommended only for low damage potential (F13) |
| Fingerprinting via open discovery | anonymous reconnaissance of which service runs where | content rule "no secrets, no internals" (F4); discovery switchable off per tenant; where desired, an obscurity token in the URL (F4) — expressly not a secret, only a curtain |
| Platform without external secret storage | secret can be supplied only via the context window | an honestly named limit: the order of precedence from F13 minimizes the exposure, it does not remove it; narrow tokens (F12) limit the damage |

Limit of the model: against a malicious service that the user names himself and trusts, no mechanism of this concept protects — the trust decision remains with the human; the concept only ensures that it is taken at one clear point (when the service is anchored) instead of diffusely with every installation.

## 6. What it costs

This section enumerates what the concept costs. Every item is fastened to the decision that causes it, and it stands here without any counter-calculation: what the concept brings in is in §1 to §4 and is not repeated at this point. Whoever knows these items in advance can later distinguish whether one of them has materialized or whether operations are facing an entirely different problem.

**Editorial work becomes an operational duty (F6).** F6 requires that every operation and every field of the interface description carry a real description and a real example, and that this duty be enforced as a check in the automatic build of the service — as a "gate in the build": a check that makes the build fail when a description is missing. Whoever follows F6 pays with a permanent item in the development flow. From now on, every new field costs a sentence of prose before it can be delivered at all, and this sentence is demanded of whoever happens to be under time pressure. Whoever switches the gate off in a pinch to push a hurried release through has effectively abandoned the decision; whoever leaves it standing pays for every hurry with writing work. The generated basis — with HTTP APIs the OpenAPI document (Source 1) — falls out of the build, its enrichment does not.

**Error texts become editorial material (F7).** F7 requires that every rejection name the correct path concretely, with the next step. This turns every error path of the service into a piece of documentation that has to be written, checked, and maintained along with the rest — not once, but with every change to the workflow. And the maintenance is not optional: an error text that names a path which has since become outdated does more damage than a meaningless rejection, because the agent who takes things literally follows the wrong instruction immediately and with conviction.

**The structure is handwork and stays handwork (F3).** F3 requires thematic parts whose axes are aligned with the consumers' concerns, and an entry page that explains this structure. Axes do not fall out of the build; they are an editorial decision about how strangers and agents approach the service. As the service grows, at some point the axes no longer fit, and the parts have to be recut. A recut does not stay with itself: part descriptions have their own addresses, the entry page lists them (F2), and the anchor skill package is divided into files along the same division (F8). Anchors already stored point, after a recut, to addresses that no longer exist in that form — the empty shell cannot go stale, its references can.

**Every description has to be computed (F5).** F5 requires that all examples be rendered with the host under which the service is currently being addressed. With that, the description is no longer a finished file on disk but an output that arises per request. Static delivery and simple caching fall away or become a special case, because the same content has two results under two hostnames. And the rendering path is itself program code that can break: an error there hits not one call but the entire self-disclosure of the service at once.

**Every session pays the loading costs anew (F1, F2).** Because nothing is installed, nothing is present at the beginning of a session. Every session that uses the service loads at least the entry page and the pertinent part description — every time from scratch. That costs the consumer context window and latency before the first call; it costs the operator deliveries whose number grows with the number of sessions, not with the number of calls. A service with many short-working agents may deliver its description more often than its payload data.

**Instructions and service fail together (F1).** Because the description comes from the service itself, its availability is the availability of the service. If the service cannot be reached, its instructions cannot be reached either — precisely in the failure case, when somebody wants to look up what would actually be normal, there is nothing to read. There is no local fallback, and under this concept there cannot be one: a copy held in reserve would be the second source that F1 excludes.

**Instructions can change without anyone noticing (F1).** Live delivery means current, and current means changeable. Between two sessions the operator can exchange every rule and every example; next time the agent loads the new text and follows it just as literally as the old one. A signal by which he would recognize the change is not provided for by this concept — whether there should be one is open (Q3). Whoever has to prove which instruction an agent followed on a particular day does not get that proof here; §7 draws the line there.

**The consumer rules are written anew at every service (F11).** F11 requires that every rule addressing consumers appear in the usage description of the respective service, in that service's own words. The consequence is multiplication: the same rules exist as often as there are services, in just as many formulations and in varying quality. Whoever uses ten services relies on ten separately written codes of conduct that may and will differ in details. This duplication is intended — otherwise no rule reaches an agent who does not know this dossier — but it is exactly the kind of copy that F1 eliminates at the level of the mechanics and F11 reintroduces at the level of behavior. Whether the dossier should supply a ready-made template text for this is open (Q8).

**Anchors and secrets multiply (F9, F12).** F9 requires an anchor of its own per instance, F12 recommends one token per capability. The two together yield an administrative burden that grows multiplicatively: whoever uses two instances of the same service with three capabilities each administers two anchors and six secrets — each with its own storage location, its own revocation, its own rotation. On top of that come the questions back: when things are ambiguous the agent asks instead of guessing (F9), and this question arises anew in every ambiguous situation, not just once.

**The open disclosure is permanently public (F4).** Whoever builds according to F4 publishes, without credentials and readable by anyone, that this service exists, what it can do, and how to obtain access. This exposed surface is not a side effect but the mode of operation, and it cannot be argued away: the content rule from F4 ensures that nothing confidential stands there, it does not ensure that nothing stands there. The only lever against it is the switch-off per tenant, and this lever costs the very property on which the concept rests: where the open disclosure is off, the service is once again an installation project for an agent that does not yet know it.

**Whoever offers self-registration operates an open endpoint (F14).** The registration method per F14 is open to anyone who knows the URL. Protecting it against abuse is expressly the operator's task: throttling, expiry of unconfirmed tokens, review of the stored self-introductions — that is ongoing operational work, not a one-time build. In the "by-request" flavor a second permanent load is added: every token created without rights waits for a human to switch it live server-side. Onboarding is then exactly as fast as the person who works through this queue.

**The façade inherits a drift it does not control (F15).** The retrofitter describes an API that does not belong to him and that he cannot change. If its operator changes an operation, then from that moment on the façade describes something that no longer exists in that form — and it does not learn of this from within itself, because it deliberately does not touch the data traffic. F15 therefore requires that the façade name the state of the target API that it describes; how it notices changes the concept leaves open (Q4). Until then, the correctness of a retrofit hangs on the attentiveness of its operator, and its failure mode is the same one F1 is meant to eliminate: a description that quietly becomes wrong.

## 7. Scope and limits

### Preconditions

For the answer of this dossier to hold, the environment has to supply four things. They are deliberately set low — that is the core of the promise — but they are conditions, not wishes: if one of them is missing, even the cleanest implementation of the decisions does not help.

**Outbound HTTP to the service.** The agent's environment must permit requests to the service's host. This carries the answer because the concept replaces installation with retrieval (F1): where nothing can be retrieved, nothing arrives, and there is deliberately no second way — the distributed copy has, after all, been abolished. You recognize the situation by the agent being unable to load the entry page at the base URL. The typical case here is not the missing line but the allowlist: sandboxes and corporate networks often permit only certain targets, and the new service is not on the list.

**Some way of remembering at the consumer's end.** The agent must be able to retain base URL and secret at least for the duration of a session. Which storage he uses for this, F10 deliberately does not prescribe — whatever his runtime environment offers anyway suffices (F10 calls it the "harness": session memory, skill catalog, whatever is present). This carries the answer because several steps lie between the user's sentence — "add this service" — and the first call; whatever gets lost in between the user has to supply again, and then the onboarding is a procedure again instead of a sentence. You recognize the situation by the symptom F10 itself names: the user has to name the URL anew every time.

**A secured connection.** The service must be reachable over TLS. This carries the answer because the self-description is text that instructs the agent: whoever can alter it in transit can instruct the agent. §5 lists this case as "spoofing / eavesdropping" and therefore sets TLS as an operating precondition. You recognize the situation by the base URL being unencrypted, or by certificate warnings being routinely clicked away in operation.

**Somebody who can deliver the description.** Either the operator controls the service far enough that it delivers the cascade itself — or somebody places a façade per F15 alongside the foreign API. This carries the answer because F1 demands delivery by the service: where nobody can deliver anything, there is not one source but none at all. You recognize the situation by the fact that nobody can be found who is allowed to add an endpoint to the service, and equally nobody who would be willing to place a description service next to it.

### When the answer tips

A dossier that is passed on is read in a situation other than the one in which it came into being. The following conditions make the answer not merely more inconvenient but wrong.

**Platform without an external path for secrets.** What is meant is an environment in which neither environment variables can be set nor files stored that outlive a call. The answer tips because F13 owes its promise to the order of precedence — environment variable before pass-through file before context window. If the first two ranks fall away, the third remains, and F13 itself calls it the last resort: the secret then stands in the transcript, that is, in a searchable and copyable store. You recognize the situation by the agent being unable to offer his user any handover path other than "type it in here for me". Whoever works there has to decide whether narrowly cut tokens (F12) carry the residual risk; with high damage potential they do not, and then the answer of this dossier does not hold there.

**Audit and reproducibility duty.** What is meant is an environment in which it has to be proven after the fact which instruction an agent followed at a particular time, or in which a run is supposed to be repeatable with the same result. The answer tips because F1 makes live delivery the only source and a live source has no past: it always shows the state of now, and what the agent read yesterday can no longer be shown from it. You recognize the situation by the question nobody can answer: "Which version of the instructions did the agent read on the third of March?" That is where the scope of this concept ends. How to freeze, preserve, and prove a state, it does not standardize — not by oversight, but because a frozen state would be a second, separately maintained description and thus precisely what F1 excludes. Whether and how the two could be reconciled is open as Q7.

**The description does not fall out of the code.** What is meant is a situation without a generated contract: an older interface without a machine-readable schema, or a façade per F15 in front of an API that does not yield one. The answer tips because F6 grounds the durability of the interface description in the fact that it falls out of the same code as the behavior of the service (Source 1). If it is instead written by hand, it is once again a separately maintained artifact that ages separately: the delivery stays live, the durability does not. You recognize the situation by the fact that nobody can say from which build step the interface description arises — it is changed in an editor, not generated.

**The open disclosure is not permissible.** What is meant is a situation in which even the existence, or the shape, of a service is confidential. The answer tips because F4 presupposes that entry page and descriptions are readable without credentials, and derives from that the resolution of the chicken-and-egg problem: the way to access must not presuppose access. Where the open disclosure has to be switched off, this problem returns; the obscurity token from F4 only shifts it too, since it must itself reach the consumer by a second path. You recognize the situation by the security policy forbidding unauthenticated endpoints on principle, not just for payload data.

**No network access for the agent.** What is meant is not the temporary disruption but the permanent separation: separated networks, offline operation. The answer tips because a service one cannot reach cannot describe itself; what would remain is only the distributed copy, that is, precisely the state §1 describes as the problem. You recognize the situation easily — harder to recognize is the intermittent case: a network that is only sometimes missing lets onboarding fail sporadically and then looks like a fault in the service.

### What it does not solve

The following neighboring questions look as though they were covered along the way. They are not.

**Finding services.** This dossier describes the onboarding of a service that somebody has named: a URL is at hand. How a user or an agent finds out at all that a fitting service exists remains outside — catalogs, directories, and federation are not part of it. The question of a standardized path convention (Q1) is expressly not the same thing: it would help to query a known host, not to find an unknown service.

**The service's rights and role model.** F12 recommends a cut for the tokens but does not replace an authorization concept: who may do what, who grants it, how it is revoked again — that remains the service's business. F14 even presupposes that there is a place server-side at which rights are granted to a display string; what this place looks like, this dossier does not say.

**The path of the secret to the user.** F13 deliberately leaves open how a secret reaches the consumer at all when no self-registration is offered. It then takes a second channel outside the chat, and its form this concept does not standardize (Q6).

**The storage mechanics at the consumer's end.** F10 expressly leaves it to the agent's runtime environment where and how anchors are stored. Whoever needs storage that is uniform across platforms — in order, for instance, to check or withdraw centrally what a user has anchored — does not get it here.

**Quality and guarantee commitments.** Availability, retention periods, audit-proof retention are not part of the cascade: the service describes what it can do and how to call it, not what it vouches for. That a design can also turn out differently is shown by the antecedent in §8.

## 8. Track record

This section separates three things that readily run into one another in concept papers: what was actually built and operated, what is being built and is still outstanding, and what merely serves illustration. Little is established so far — one precursor in operation, one reference implementation in progress, one antecedent from a different context. Every piece of evidence therefore stands in the same grid: what it is, what it establishes, what its status honestly is, and where it comes from. The worked example at the end is invented; it establishes nothing, it only shows how the decisions mesh.

### Evidence 1 — Reference implementation: the mail filing service

**What it is.** A service by the author that files mail from a mailbox and makes it findable again, built according to the builder decisions of this dossier: a cascade of entry page, usage description, and interface description, the latter as enriched OpenAPI with an enforced description duty; teaching error messages; open discovery, switchable off per tenant; an anchor skill package whose usage description prescribes the scope question to the user; narrowly cut tokens with a non-secret handle, the display string.

**What it establishes.** Touched are F2 (entry page and cascade), F4 (open discovery, switchable off per tenant), F6 (pumped-up contract including the enforced description duty), F7 (teaching error messages), F8 and F10 (the anchor skill package and the question about the scope), F11 (this question appears in the service's own usage description, not only in this dossier), as well as F12 together with the display string from F14. What is thereby established so far is one thing: that these decisions can be specified out completely and without contradiction for a real, non-constructed service. What is not established is operational experience — no consumer has used this service in everyday work.

**Status, honestly.** The specification is complete. The implementation by a development agent is under way as of this draft (13 August 2026); a completion report was still outstanding on the evening of writing and will be added here. Self-registration per F14 is envisaged as a later stage and is thus expressly not yet part of the evidence.

**Source.** Source 11.

### Evidence 2 — Precursor in operation: the wiki toolkit

**What it is.** A toolkit by the author that makes the wiki platform Wiki.js usable for agents via its GraphQL interface (Source 9) — but by the old route: as a distributed skill, not as the service's self-description. It serves several wiki instances at the same time and follows a strict secret doctrine: environment variable before file, with a hardened pass-through, so that the secret never appears in an output. With it, a wiki with 140,000 nodes was built — an order of magnitude that shows the route was not tried out on a toy case.

**What it establishes.** Two things, in opposite directions. First, positively: F9 (several instances of the same software, each treated on its own) and F13 (secret order of precedence) are practicable under load, and the curation of the operations — the core of F6 — has proven to be the actual benefit, not the completeness of the schema. Second, negatively, and therein lies the more valuable evidence: there, the curation sits in a *separate* file next to the complete schema. Three artifacts age side by side — the generated schema, the curated operation list next to it, and the distributed skill file that brings both to the consumers. In precisely this set, the early indicator from F1 actually occurred: the number of canonical operations read "12" in one artifact and "13" in the neighboring document. Both numbers looked plausible, neither was recognizable as wrong, and an agent who takes things literally would have built, out of the one version, a call that the other does not know. This case is the reason why F6 moves the curation *into* the carrier instead of next to it, and why F15 offers the façade as a way to deliver the same value as a service instead of as a distributed file.

**Status, honestly.** In operation and in daily use — but expressly a **precursor, not a model**: the toolkit is the distributed copy that F1 abolishes. It establishes the problem as much as the value of curation; the solution of this dossier it does not establish.

**Source.** Source 10; on the platform described, Source 9, on the mechanics of its interface, Source 6.

### Evidence 3 — Antecedent: the self-description services of 2007

**What it is.** The author described the principle in 2007 in his dissertation, for a peer-to-peer system with grid functions: there, every service had to provide a self-description service that, besides the methods offered, also stated their *quality* — for instance whether a storage service is audit-proof and how long it guarantees the availability of stored data.

**What it establishes.** That the core of F1 — the service describes itself at runtime instead of letting distributed copies describe it — is not a novelty of this dossier but has already been carried through in another system. It expressly establishes nothing about the present design: environment, consumers, and purpose were different, and the quality dimension of that time is deliberately absent here (§7, "What it does not solve").

**Status, honestly.** Completed work from 2007; not continued, not carried over into today's design. It serves as a record of the idea's origin, not as a track record.

**Source.** Source 12.

### Worked example (entirely fictitious): the traffic service

What now follows is **not evidence**. It is the invented traffic service from §2.5, played through once from beginning to end, so that it becomes visible what the decisions look like in interplay. All values — names, addresses, tokens — are fictitious; none of the sessions described took place.

The user says: *"Please install `https://verkehr.example.org` for me permanently."* The agent loads the entry page at the base URL — twenty lines: the purpose ("learn the traffic situation of a region and contribute reports"), the four parts with one sentence each, the active skill offer ("You can save or install this service as a skill: …"), the pointer to self-registration, and the canonical variable name `TRAFFIC_API_TOKEN`. The agent fetches the anchor skill and stores it permanently with the means of his runtime environment — nothing more is installed. Visible here are F2 (the entry page suffices for the decision), F8 (the entry page actively holds out the anchor skill), and F10 (the user determines the scope "permanently" in natural language).

For access, the agent calls self-registration, with a self-introduction: *"Agent of J.; purpose: report and retrieve traffic-jam reports on the commute."* The answer names the flavor **by-request**, a token, and the display string `kluvmoth`. The agent stores the token according to the order of precedence from F13 — in the environment variable, in a sandbox if need be as a pass-through file — and tells his user only: *"Please grant the access **kluvmoth** the reporting rights."* The user switches `kluvmoth` live server-side. The secret itself never touched the chat; visible are F14 (self-introduction, flag, display string) and F13 (order of precedence for storage).

Days later: *"Report the traffic jam on the A 9."* The anchor skill triggers; the agent loads **only** the part description "contributing reports" (F3) and obtains — as the usage description prescribes to him (F11) — the user's confirmation before he does anything that takes effect. On his first attempt he embeds the traffic-jam photo in the report and receives the teaching error message from F7: it names not only the rejection but the way — first upload the file, then reference the returned `file_id`. He does exactly that, and the report goes through.

Weeks later: *"Add the new charging station as a POI."* From the short version on the entry page the agent has known since day one that there is a POI part — he has never read it. Only now does he load its description. And when the user adds the traffic service of his holiday destination, a second anchor arises with its own base URL and its own secret; to *"report the traffic jam"* the agent asks back from then on: *"In your city or at the holiday destination?"* That is F9 in operation — and at the same time the point at which §6 presents the bill: two anchors are two anchors.

## 9. Open questions

**Q1** Does the cascade need a standardized path convention — a reserved path prefix in the style of the well-known URIs (Source 5), for instance — so that an agent can query an arbitrary host generically as to whether a self-describing service answers there? Or does what F2 lays down suffice permanently: whoever knows the base URL reaches everything else from there, and a path convention remains a comfort instead of a condition?

**Q2** Should expiry periods for unconfirmed tokens and minimum values for throttling be standardized for self-registration — or does protection of the open endpoint against abuse remain entirely the operator's business, as F14 leaves it today? Standardization would make the protection comparable and checkable; but it would prescribe to operators an operational quantity whose right value only they know.

**Q3** Does the concept need a recommendation on how often an agent reloads the description, and a version signal from the service by which the agent recognizes — and reports to his user — that the instructions have changed since the last use? Without such a signal, the changeability named in §6 goes unnoticed, and the countermeasure in §5 for the reload problem rests on the user's confirmation duty alone.

**Q4** How does a façade (F15) reliably recognize that the target API has changed — via a version anchor that the target API itself states, via regular spot-check calls, or via a promised notification duty on the part of its operator? And by what path does a consumer who has already anchored the façade learn of it?

**Q5** What are the concrete enrichment rules per interface dialect? For OpenAPI (Source 1) they are spelled out in F6; for GraphQL with its introspection (Source 6), for JSON-RPC with OpenRPC (Source 8), and for gRPC with its reflection (Source 7), the appendices announced in F6 remain to be worked out — together with a sentence that carries the analogy for further dialects, without each one having to be treated individually.

**Q6** F13 deliberately leaves the second channel from operator to user informal when no self-registration is offered. Does this need a recommended handover protocol — a one-time link with an expiry, for instance — or is the informality the more honest norm, because any recommendation would suggest a security that a channel outside the service does not have?

**Q7** Audit and reproducibility: §7 draws the line where a dated, frozen state of the description is demanded — this concept does not standardize it, because such a state would be a second, separately maintained description and would thus contradict F1. Should it stay that way, or does a later version need a named exception: a dated copy delivered by the service itself, expressly not authoritative but serving only as proof? Before including it, it would have to be clarified who produces the copy, how long it is kept, and how a consumer recognizes that he has a copy and not the live source in front of him.

**Q8** F11 obliges every builder to repeat the consumer rules in his own usage description and in his own words; §6 names the costs of this multiplication. Should this dossier supply a ready-made template text to adopt for that — and would such a template text be a help or once again a distributed copy in the sense of F1, only one level higher?

## 10. Sources & related work

This is the only place in this dossier where references stand; the running text of sections 6 to 9 points here with numbered source references. Every entry names title, locator, and the purpose for which it stands here. The locators were checked on 14 August 2026; where checking was not possible, this is noted.

**Source 1 · OpenAPI Specification, version 3.2.0 (19 September 2025).** https://spec.openapis.org/oas/latest.html — *What it stands for here:* it is the "generated contract" from §2.1 that F6 declares a product and enriches; its description and example fields are the places the gate from F6 demands. It also supports the tipping condition in §7, according to which the durability of the interface description depends on its falling out of the code.

**Source 2 · Model Context Protocol.** https://modelcontextprotocol.io/ — *What it stands for here:* the model for the parallel in §2.2 — a server describes its methods at runtime, the client queries them (discovery instead of distribution). This dossier carries the same principle over to the web API, which needs no connecting.

**Source 3 · Roy Thomas Fielding: *Architectural Styles and the Design of Network-based Software Architectures.* Dissertation, University of California, Irvine, 2000 — chapter 5, "Representational State Transfer (REST)".** https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm — *What it stands for here:* the origin of the hypermedia idea that F7 invokes: responses that name the next steps themselves. F7 redeems this old promise for a consumer who actually does read at runtime.

**Source 4 · Jeremy Howard: *The /llms.txt file* (version v2).** https://llmstxt.org/ — *What it stands for here:* the closest existing convention for agent-readable self-disclosure, and thus the benchmark for the entry page from F2. It shows Markdown with links as the carrier form; this dossier goes beyond it in that the disclosure is rendered by the service at runtime (F5) and loaded in stages (F2).

**Source 5 · RFC 8615: *Well-Known Uniform Resource Identifiers (URIs).* M. Nottingham, May 2019, Standards Track.** https://www.rfc-editor.org/rfc/rfc8615 — *What it stands for here:* the reference point of Q1: it reserves the path prefix `/.well-known/` and is thus the pattern on which a possible path convention for the cascade would be modeled.

**Source 6 · GraphQL: Introspection.** https://graphql.org/learn/introspection/ — *What it stands for here:* native self-description of the mechanics in another dialect, including description fields on the schema — the point of attachment for the analogy from F6 and the subject of Q5. It is at the same time the interface mechanics of the wiki platform from §8.

**Source 7 · gRPC: Reflection.** https://grpc.io/docs/guides/reflection/ — *What it stands for here:* native self-description of the mechanics with gRPC; a service discloses its protobuf definitions via a standardized call. Completeness of the mechanics without curation — precisely the gap that F6 fills; subject of Q5.

**Source 8 · OpenRPC.** https://open-rpc.org/ — *What it stands for here:* the contract mechanism for JSON-RPC 2.0 that F6 names as a dialect to be enriched analogously; its `rpc.discover` is the counterpart there to the runtime query. Subject of Q5.

**Source 9 · Wiki.js: GraphQL API (developer documentation).** https://docs.requarks.io/dev/api — *What it stands for here:* the platform to which the precursor in §8 refers, and the type of existing API for which F15 provides the façade — a complete GraphQL interface without a curated entry for agents.

**Source 10 · The author's wiki toolkit, repository `syncwiki`.** https://github.com/joern-schlingensiepen/syncwiki — *What it stands for here:* Evidence 2 in §8 (precursor in operation, 140,000 nodes, the count contradiction "12" next to "13").

**Source 11 · The author's mail filing service, repository `imap-service`.** https://github.com/schlingensiepen/imap-service — *What it stands for here:* Evidence 1 in §8 (reference implementation).

**Source 12 · The author's dissertation, 2007: self-description services with quality attributes in a peer-to-peer system with grid functions.** *[Bibliographic details to be supplied by the author — title, university, locator.]* — *What it stands for here:* Evidence 3 in §8 (antecedent) and the origin of the quality dimension that §7 expressly does not cover.

**Source 13 · Sister dossier of this series: *capability-tokens-for-agents* — in preparation, not yet published, hence without a locator.** — *What it stands for here:* the deepening of F12 and F13: justification and handling code for the narrow token cut. A deepening, not a must for understanding — the core rules needed here are summarized self-sufficiently by F12 and F13.

**Placement.** The conceptual line of this dossier runs between two traditions. The one is hypermedia (Source 3): responses that name the next steps themselves. It came too early, because human developers did not want to discover anything at runtime; agents do it without complaint, and F7 redeems the promise late. The other is the generated self-description of the mechanics in individual dialects (Sources 1, 6, 7, 8): it delivers completeness, but no weighting — the consumer learns what exists, not what belongs together and what has which consequences. This dossier adds in three places: curation *inside* the carrier instead of next to it (F6), a behavioral level alongside the mechanics (F11), and a staged entry that puts the decision before the loading (F2). Source 2 shows the same principle for tools, Source 4 a leaner form of the same idea without runtime rendering.
