# Proposals

A library of technical and organizational proposals that lets the user of an AI agent bring pre-packaged best practice into their project. Each proposal puts a proven concept up for discussion and can be taken, unchanged, by an agent as a work instruction; where more than one answer is right, it names the options and lets the user decide. Each proposal is one directory and one [OKF](https://github.com/GoogleCloudPlatform/open-knowledge-format) bundle: start with its `README.md` (question, thesis, identity card), then `index.md` (one line per file). Builders adopting a proposal go to its `AGENT.md`.

**Agent entry:** [AGENTS.md](AGENTS.md) routes to two jobs: *create or maintain* a proposal · *find and use* one.

**Language:** everything in this repository is English. The vocabulary of the series was built in German; every bundle carries a `terms.md` with each term in both languages and a short explanation of each.

## Register

The register is the only place that states what a proposal is and in which state it is. Bundles never say this about each other; they name each other by slug only.

### Base concepts

**[`service-as-single-artifact`](service-as-single-artifact/)** · Proposal · draft · v1.2 (2026-11-01) — One service, one function, one artifact with several modes, one deliverable; configuring the function and issuing access are one act, so roles, rights management and admin never arise.

**[`one-configuration-one-judgment`](one-configuration-one-judgment/)** · Proposal · draft · v1.0 (2026-10-01) — One configuration, judged once by the code that will run on it; every value names where it came from, and an empty or unusable value is a reported error, never a silent default.

**[`where-a-secret-comes-from`](where-a-secret-comes-from/)** · Proposal · draft · v1.1 (2026-11-01) — One ranking of sources for every start secret and every role, environment first, then a pass-through file, last a channel used exactly once, where the higher rank wins but never hides a lower one, and replacing is two moves.

**[`capability-tokens-for-agents`](capability-tokens-for-agents/)** · Proposal · draft · v1.1 (2026-10-13) — A token embodies exactly one capability at exactly one place, and its handling follows fixed rules along the lifecycle, from creation and handover through storage and use to rotation, revocation and expiry, so that an agent context in which every leak is searchable and copyable survives partial failure.

**[`self-describing-services`](self-describing-services/)** · Proposal · draft · v1.1 (2026-11-01) — A service that describes itself at runtime, in staged and linked texts its weakest intended reader can accept, makes installation unnecessary; the agent keeps only a URL and a secret, and because there is no second source, nothing goes stale.

**[`invitation-instead-of-login`](invitation-instead-of-login/)** · Proposal · draft · v1.0 (2026-08-15) — Access arises from an invitation; one link invites a person for all their devices because every redemption creates its own credential, and machines receive capability rather than identity.

**[`single-endpoint-json-rpc`](single-endpoint-json-rpc/)** · Proposal · draft · v1.0 (2026-08-14) — The entire API is a single POST endpoint with a JSON-RPC 2.0 envelope; credentials go in headers, binary data stays outside.

**[`single-file-static-frontend`](single-file-static-frontend/)** · Proposal · draft · v1.0 (2026-08-15) — The frontend is one artifact, an HTML file with its vendored assets, delivered without authentication and therefore allowed to contain no data; everything of substance it fetches at runtime through an API.

**[`get-only-exit`](get-only-exit/)** · Proposal · draft · v1.0 (2026-10-13) — A named emergency exit for callers that can only send GET without headers of their own; the same operations under their own prefix, everything in the path with the secret first, writing as small fetches of which exactly one takes effect, and the price, the secret in the address, named at the door.

**[`one-operation-many-surfaces`](one-operation-many-surfaces/)** · Proposal · draft · v0.1 (2026-11-01) — Every administrative action exists exactly once as an operation that knows no surface, every surface of the service is a pass-through onto it with only translation and presentation of its own, and a structural test in the build holds that, because two correct copies pass every behavioral test.

**[`proven-from-outside`](proven-from-outside/)** · Proposal · draft · v0.1 (2026-11-01) — A check proves something only once it has been seen to go red with its safeguard removed, and a document meant to work without inside knowledge counts as accepted only once a reader without that knowledge has completed a task with it.

**[`build-stamp`](build-stamp/)** · Proposal · draft · v0.1 (2026-10-13) — The build stamps the source revision into the artifact itself, the artifact reports it when asked against the running instance, and the build pipeline publishes nothing that cannot report exactly the revision it was built from.

**[`push-deployment-that-keeps-its-secrets`](push-deployment-that-keeps-its-secrets/)** · Proposal · draft · v1.0 (2026-08-15) — Each component is delivered by a versioned script steered by one directory per instance outside version control; the script creates and places the system's secrets itself, and because one part only gets its access while another holds a window open, the order of the partial deployments follows from the flow of secrets rather than from an agreement.

**[`pull-deployment-from-a-registry`](pull-deployment-from-a-registry/)** · Proposal · draft · v1.0 (2026-10-13) — In a pull deployment nobody stands at the target, so every assurance of the chain sits either before publishing, in a build that runs the image against itself, computes its names and measures its visibility, or in what the operator holds, a deployment file whose header says what they pull and a call that tells them what is running.

**[`maildir-style-file-queues`](maildir-style-file-queues/)** · Proposal · draft · v0.9 (2026-08-15) — A directory tree is the queue; every job is a file, its state is the folder it lies in, and every change of state is an atomic rename on the same file system, lock-free, crash-safe and inspectable with the tools of the operating system.

**[`append-only-state-history`](append-only-state-history/)** · Proposal · draft · v0.2 (2026-08-15) — The state of an item is never changed but replaced; every change creates a new, complete state, and the history is the sequence of these states.

**[`self-contained-repos`](self-contained-repos/)** · Proposal · draft · v0.9 (2026-08-15) — A repository is self-contained when someone opening it for the first time, human or agent, can work from it alone; an entry point that branches to the actual tasks, decisions with their reasons in full text, and a check rule asking whether everything still holds when every external link is dead.

**[`two-repo-submodule-pattern`](two-repo-submodule-pattern/)** · Proposal · draft · v0.10 (2026-11-01) — What arises while something is being made and what is finally published belong in two repositories with two histories, connected as a submodule or, where the tools do not allow that, as two independent roots with an ignore rule.

**[`immutable-decision-records`](immutable-decision-records/)** · Proposal · draft · v1.0 (2026-08-13) — An accepted decision record is never changed again; every change is a new, complete record that replaces the old one, and both ends of the replacement are kept.

**[`decision-records-with-madr`](decision-records-with-madr/)** · Proposal · draft · v1.0 (2026-08-13) — A decision record follows MADR 4.0.0 unchanged; the only extension is two list fields in the head recording what this document supersedes and what superseded it.

### Extensions

**[`admin-surface-over-http`](admin-surface-over-http/)** · Proposal Extension · draft · v1.0 (2026-10-13) — Where the operator has no shell, a service built as one artifact gets a complete second administration surface over HTTP beside its commands, on its own port, off as shipped, with a root secret it generates and shows once, and both surfaces pass through to one operation.

### Planned (no bundle yet)

`service-as-a-container` (draft from two deployments, awaiting the author) · `redacting-generated-documents` · `git-safety` · `teaching-error-messages`.

## Layout of a bundle

One directory per proposal; descriptive English slug, no numbers. Every Markdown file except `index.md` and `log.md` carries a YAML head with `type` and a one-sentence `description`. Links are relative and stay inside the bundle; references to other proposals are slugs, resolved through this register.

| File | What it holds |
|---|---|
| `README.md` | Identity card (type, version, status, author, `extends`, `imports`) and facade: question, thesis, where to start. |
| `index.md` | The map: every file with one line, grouped so that a reader can decide what to load. |
| `concept.md` | Problem, the concept in prose, the procedure. |
| `terms.md` | The bundle's terms in English and German, each with a short explanation. |
| `rules/F<n>-….md` | One rule per file: *what holds* · *why* · *obvious and rejected* · *how you notice it is violated*. Numbers are stable. |
| `choices/W<n>-….md` | One choice per file: options with consequences and conditions; the concept does not decide, the adopter does, and records it. |
| `scope.md` | Security considerations, costs, where it applies, when the answer flips, what it does not solve, what is not yet settled. |
| `evidence.md` | Where it has held. |
| `sources.md` | What the proposal builds on: imports, related proposals, external sources. |
| `used-by.md` | Which proposals import or extend this one; generated. |
| `extensions.md` | Proposals that extend this one, with what they lift or tighten; generated. |
| `AGENT.md` | Self-contained guide for an agent adopting the concept, including the duty to decide the choices with the user. |
| `log.md` | Dated changes. |
| `examples/` | Optional: requests, test cases, templates. |

A copyable skeleton is in [`_template/proposal/`](_template/proposal/).

## Types, relations, status

**Types** (the `type` of the bundle's `README.md`): `Proposal` is a base concept, complete on its own; `Proposal Extension` builds on one or more base concepts and names them in `extends`, lifting or tightening some of their rules. Both are bundles at the root of this repository; the difference is the type, not the place. Compendia (curated factual knowledge with a survey date) are not proposals and are not part of this library.

**Relations** (fixed vocabulary): on what a proposal is built: `imports` (presupposes, does not repeat), `extends` (an extension builds on it), `form-of` (gives a mechanism a form), `generalizes`, `supersedes`; on use together: `recommended-with`, `overlaps-with`, `tension-with`, `alternative-to`, `excludes`; on what an extension does to a rule of its base: `tightens`, `lifts`. A bundle lists what it builds on in `sources.md` and is told what builds on it in `used-by.md` and `extensions.md`, both generated from this register.

**Rules and choices:** a rule has one answer; whoever gives it up is no longer using the concept. A choice has more than one legitimate answer; the proposal names the options, their consequences and when each fits, and the adopter decides, per setting where that matters, and records the decision. An agent adopting a proposal must put the choices to its user instead of picking silently.

**Status:** draft → stable (only with reference evidence in `evidence.md`) → deprecated (with a successor named in `log.md`). Transitions and version changes are recorded in the bundle's `log.md`, never retroactively.

## Self-containment

Each bundle is understandable and applicable without the others, except for what it names in `imports`: an imported concept is presupposed and fetched from this repository, not repeated. All other cross-references are slugs and sources, never prerequisites; what a reader needs is in the bundle, even at the price of redundancy. Test: does the bundle still work when every link outside it is dead?

## Discussion

Objections and contributions per proposal via issues or discussions, addressed by slug and rule or choice number (`where-a-secret-comes-from F4`, `service-as-single-artifact W1`). Accepted objections become a new version and an entry in the bundle's `log.md`.

## License

[CC BY 4.0](LICENSE). Attribution in the text is "the author"; the `author` field in each bundle's head carries the name, and every bundle's `README.md` names the series and this license.
