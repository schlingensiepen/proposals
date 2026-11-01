---
type: Proposal Concept
description: The problem of giving agents a service without installation, the concept of a service that describes itself at runtime in a staged, linked cascade that arrives at its weakest reader and answers even before it is set up, its load-bearing structure, and the two procedures for builder and consumer.
---

# The concept

## Problem

The author asked himself this question as a **user**: there is a service — a mailbox filing service, a wiki, a domain API — and there are agents on various platforms, in cloud sandboxes, in the terminal, in the browser. Why is "please use this" an installation project? For every platform a connector, for every connector an installation, for every installation permission dialogs — and in the end the agent still knows only what somebody wrote down for it weeks ago.

Four roles carry the problem together:

**The user** wants to be able to say "add this service" — as a sentence, not as a project. He has several agents on several platforms, and many of them run in sandboxes where simply nothing can be installed. What an agent can do there is: speak HTTP and remember something. An onboarding that demands more excludes precisely those environments in which agents increasingly work.

**The operator** of a small or internal service cannot build, publish, and keep current a connector for every agent platform. Every distributed description of his service — skill file, connector, wiki page — is a copy he will never fully round up again: this is an N-services-times-M-platforms problem, and it grows with every release.

**The retrofitter** faces a foreign existing API that he neither owns nor can change — a wiki, a ticket system, a domain API — and wants to make it usable for his agents. All that remains to him today is the distributed skill file: a copy of someone else's knowledge that grows quietly wrong with every update of the target service and that he has to deliver to each consumer individually.

**The agent**, finally, believes its instructions literally. A human developer grows suspicious when the documentation does not match the API, and starts experimenting; an agent produces perfectly formatted, wrong calls from an outdated description — with confidence. For him, instructions that *can* be outdated are instructions that will *lie* at some point.

The usual way out — more discipline in maintaining the documentation — fights the symptom. The structural answer starts elsewhere: installation and copying are the problem. What is not distributed does not have to be installed; what does not exist separately cannot go stale separately.

## The concept

### The starting point: interfaces almost describe themselves already

A web interface starts out doing something very ordinary: it offers operations — read data, write data, accept files — and precisely this **mechanics** is today largely documented automatically. Modern frameworks generate the interface description directly from the code: with HTTP APIs, **OpenAPI** is the established form — routes, fields, types, status codes fall out of the build without being maintained, and they sit machine-readable at a URL of the service itself. That is an excellent starting point: the contract already exists, it is already generated, it is already retrievable. All that is left is to **adapt it to be agent-friendly** — to "pump it up": every field described, every example real, every error explained — and the formality turns into a function.

### The parallel: what MCP is for tools, the web API becomes for services

The Model Context Protocol has demonstrated what agent integration without knowledge distribution looks like: an MCP server describes its many methods **at runtime**, the client queries them — discovery instead of distribution. This concept carries exactly that principle over to the web API. The decisive difference: an MCP server first has to be *connected* at the consumer's end — an entry, a process, a permission. The web client, by contrast, every agent has built in long since; HTTP is the one capability that is present everywhere, right into the locked-down sandbox. So all we are doing is moving the self-description of an MCP onto the interface that needs no connecting.

### Two kinds of description — and a deliberate breach

Knowledge about a service falls into two kinds. The **interface description** carries the mechanics: what exists and how to call it correctly. The **usage description** carries the overarching behavior: what the service is meant for, in what order one works, which duties apply toward the user, how access is to be handled. But the clean separation is only the beginning — we **breach it on purpose**: when pumping up, every piece of behavior that clings to a single operation ("this call is asynchronous", "this needs your user's confirmation", "this error means: upload first") migrates *into the interface description*, right next to the operation. The gain is twofold: the agent finds the rule exactly where he builds the call — and the usage description stays small, because it now carries only what is genuinely overarching. The breach has one condition: no rule stands in both places. The usage description names the principle once, the operation its case, and because the sentence that wants into the usage description is useful every time, the separation holds only by machine, with a check in the build (F22).

### The entry page: hypertext, rendered at runtime

Everything begins at a single address: the **entry page** at the base URL is an unauthenticated short description of some 20 lines — *and* the directory of everything else. It says what the service is and how to obtain access, and it **links every part of the description**: the usage description, the interface description, the thematic part descriptions (each with one sentence on what the part does), the anchor skill. This is the old strength of hypertext, applied to self-disclosure: you need to know nothing but one address — from there everything is reachable, and at every link it says what awaits you behind it. Whoever builds such an entry page inserts the URLs **at runtime**, with the host under which the service is currently being addressed — which makes every example copyable for exactly this installation. The sequence "short description – link – long description – link – …" is not a stylistic device but **context-window thrift**: at each step only what the next decision needs is loaded — the rest stays a link. And hypertext is meant here as a **concept**, not as a format: the steps are as a rule Markdown with links — immediately readable for agents and humans alike; HTML is not needed for it. They are delivered in a form that even the weakest intended fetch tool accepts; F16 says what that means.

### A walkthrough: the traffic service

What this feels like is shown by an invented service that accompanies us through the whole dossier *(entirely fictitious and cut for teaching purposes; the real track record is in the evidence, and the walk-through is in the examples)*: a service that **collects and provides traffic data**. Its entry page divides the interface into four explained parts: **learning the traffic situation** (the frequent read case) · **contributing reports** (the write case) · **uploading data sets** in various formats (rarely needed, extensive) · **filing and retrieving POIs** (static points of interest — useful, but not in every use case). An agent never comes "just because"; he comes with a concern: he wants to *learn* something or to *contribute* something. At the entry page he takes the branch and loads **only** the part description for his concern — the upload formats and the POI administration he does not even read, and he does not clog his context window with them. But because he knows the short version, he knows forever *where* he has to look if his user does want to maintain POIs next week.

And the walkthrough shows one more thing: **a service can occur several times in a user's life.** The traffic service of one's own city and that of the holiday destination, the company wiki and the private one — same software, same API, but each with its own base URL, its own rights, its own secret. What is remembered is therefore always the **instance**, never "the software"; what that means in detail is governed by F9.

### Building instructions — and retrofitting along with them

The rules are **building instructions**: they say how one *writes* such a self-describing interface. And they apply twice over from the very start. Either you build a **service of your own** and deliver the description out of it — or you **retrofit a foreign existing API**: a small description service of your own, the **façade**, delivers the complete cascade for the foreign API and tells the agent "execute the operations described here directly against the original service". The façade touches no data traffic; it makes an API that does not describe itself *immediately usable*. Building and retrofitting are the same instructions with a different place of delivery — which is why the façade appears here at the beginning as an application and at the end (F15) as a rule.

### Access: in every case a secret — sometimes it is a token

That leaves access. For a **new build** we recommend the token model: narrowly cut tokens, each embodying exactly one capability (F12). With an **existing API**, the authentication brings along whatever it has — an API key, a password. Either way: **in every case you have a secret; sometimes it is a token.** And for every secret, whatever its origin, the same ranking of sources applies, the one `where-a-secret-comes-from` lays down: environment before pass-through file before once-only channel, and for the consumer the once-only channel is the context window. This concept imports that ranking, and F13 says who prescribes it to the consumer.

### The chain of transmission: who reads this dossier — and who does not

A consuming agent does not know this dossier, and he does not need to: he knows only the service's self-description, and that is self-contained. Therefore every rule of this dossier that addresses consumers is in truth a **delivery duty of the builder**: it must appear in the service's own usage description, in the service's own words. The chain reads *dossier → builder → self-description → agent* — this document instructs the one who builds; the service instructs the one who uses (F11).

### Arriving: form, present tense, closure, cut

A description is only of use if it arrives at the reader. **First:** a description the reader cannot *accept* is none. The service answered completely and correctly, and several fetch tools discarded the answer by its media type; the delivery form belongs to the description, not to the packaging (F16). **Second:** a text read as an instruction knows no past tense and no inside knowledge. What it names is imitated, whether "exists" or "existed" stood next to it; what it mentions, the reader must be able to resolve with what he has; and every word may mean only one thing (F17 to F19). **Third:** texts are cut, and the entry page links informatively (F20). The incidents behind this are in the evidence.

The checks this concept demands (F6, F17, F18, F22) and the reading by somebody without inside knowledge (F19) hold only once they are proven from outside; how, follows the imported concept `proven-from-outside`.

### Before setup: the empty state

A self-describing service is meant to be opened up from its URL. In the first version of the reference implementation the cascade was bound to the configuration: as long as no mailbox had been set up, the entry page answered 404. That reliably shuts out every agent and is the exact opposite of the purpose: **a service that does not describe itself before it is set up cannot be onboarded from its URL**, and whoever is to set it up finds nothing that tells them what they have in front of them. Nor is this an exception: an image that has just been pulled from a registry and started *is* this case, every time, at every instance, as the normal state. If a delivery procedure describes how a service comes up without anybody's help, what the service says about itself in the minute after that belongs to it.

Self-description therefore has a third state between "does not exist" (404) and "here is the description": "exists, but is not set up yet", the **empty** state. Whoever does not decide it decides it by accident, and usually in favor of 404, because that is what falls out of the configuration logic; then the description is missing at the one moment somebody needs it in order to set the service up (F21).

The reference implementation decided as follows: as long as no mailbox has been set up, the entry page delivers a **setup page** with status 200 instead of an error. It says what the service is, that it is not set up yet, and what would have to be done next; only with the setup does the regular self-description take over. The whole cascade can be switched off with an operating parameter; whoever switches it off gets 404 again. This is one possible answer, not the only one: a service in a closed network may decide otherwise. What F21 fixes is that the empty state is decided, not in which direction.

Each direction has its price. An instance that is not set up yet gives anonymous information about itself: which software runs here, and that it is unfinished. That is service fingerprinting with an addition: "something fresh and not yet configured is standing here" is worth more to an attacker than the bare identity of the software. In the reference implementation the risk was known, weighed and carried deliberately; the reasoning was that a service nobody can connect to has no use against which a residual risk could be weighed. The price of the other direction is the incident above: no onboarding from the URL before setup.

**Load-bearing structure.** F1 carries everything. F2–F7 are its concretization for the builder: cascade and division (F2/F3) make the one source loadable, F4/F5 make it reachable and literally usable, F6/F7 fill it — in the contract and in the error case. F8–F11 are the consumer side: without an actively offered anchor there is no triggering (F8/F10), without instance discipline no safe target (F9), and without the delivery duty (F11) no consumer rule ever reaches an agent. F12–F14 carry independently — the access gear holds even without the rest. F15 extends F1 to foreign services and inherits its drift risk in measured form; if F15 falls, the building principle for one's own services remains untouched. F16 to F20 are conditions for F2 to F7 to *arrive* at the reader at all: a cascade discarded by its media type, removed forms that are named and imitated, identifiers that do not resolve, words that mean two things, and texts too long to load before the decision turn the one source into a false source; F1 then holds formally and lies all the same. F21 extends the promise that the service can be opened up from its URL (F2, F4) to the first call, before anything is set up. F22 holds the deliberate breach between the two kinds of description: without the separation held by machine, the second description returns through the back door.

## The procedure

The concept has two procedures: one for the builder, one for the consumer. They fix the order and the results, not language, framework or interface dialect.

### Builder: build order

1. **Generate the machine contract from the code** (OpenAPI or the dialect's equivalent) and wire the build gate: the build fails on any missing description or example. (F6)
2. **Enrich the contract and cut the description:** real descriptions, real examples; split the description into thematic parts along what consumers come to do, each text cut so that every step loads only what the next decision needs; explain the axis of the split. (F6, F3, F20)
3. **Write the usage description:** purpose, workflows, and every consumer duty: confirmations, upload flow, obtaining the secret by the imported ranking under the canonical environment-variable name. (F11, F13)
4. **Build the entry page** (about twenty lines) that links everything and says at every link what lies behind it; add live templating so that all URLs and examples render with the request's host. (F2, F5, F20)
5. **Set the delivery form:** every prose step `text/plain` by default, the exact type only on explicit `Accept`, `Vary: Accept` on every response; state in the description which form the service's answers take. (F16)
6. **Write teaching error responses** for every rejection path. (F7)
7. **Wire the prose gates:** no hindsight, no internal identifiers, every placeholder introduced together with the call that yields it, in the same sentence. (F17, F18)
8. **State every behavior rule once:** the principle in the usage description, the case at the operation; a check in the build fails as soon as the usage description takes in operation behavior. (F22)
9. **Decide the empty state with the user:** what the entry page answers before anything is set up, a setup page with 200 or a 404, and record the decision with its price. (F21)
10. **Package the anchor skill** (empty shell, files per part) and add its active offer to the entry page. (F8)
11. **Set up token issuance:** narrow tokens, display strings, operator-side creation; optionally the self-registration endpoint with flavor, throttling and expiry. (F12, F14)
12. **For a retrofit,** do all of the above in a separate façade service against the foreign API, and state the target version it describes. (F15)

Before a surface is published, somebody without inside knowledge reads it for double meanings; every comprehension question counts as a finding. (F19) That the checks of steps 1, 7 and 8 test anything, and that this reading is done from outside, is proven as `proven-from-outside` lays down.

### Consumer: onboarding to first call

1. The user says "make `<url>` available `<scope>`". Load `GET <url>`, the entry page, about twenty lines. (F2, F10)
2. Fetch the anchor skill offered there; store it at the named scope with your runtime's own means. Nothing else is installed. (F8, F10)
3. If access is needed: use self-registration if the entry page offers it; send a short self-introduction naming your user and purpose; store the returned token per the ranking; tell the user only the display string. Otherwise ask the user to provide the secret over a channel outside the chat, ideally into an environment variable under the announced canonical name. (F14, F13)
4. On a matching task later: load the one relevant part, follow the duties of the usage description (confirm before actions that take effect), make the call. (F3, F11, F20)
5. On an error: read the response as an instruction; it names the correct next step. Follow that step instead of improvising. (F7)
6. On a second instance of the same service: create a second anchor with its own secret; from then on, ask which instance is meant whenever a request is ambiguous. (F9)
