---
type: Proposal Evidence
description: Where the concept has held; the draft-filing service specified exactly according to this cut, walked through from beginning to end, and the origin of the dossier.
---

# Evidence

> This evidence describes the reference service as it was specified for version 1.0 of this proposal. Version 1.1 moves the variants (there: the accounts) out of the configuration into the store (F12), a move the reference implementation made after the incident F12 describes; and it requires a hash of the access token (the reference stores an unsalted SHA-256 of a high-entropy token, which F9 permits for secrets no human chose).

This section shows what has been built and separates it from what is still outstanding. Unlike in the sister dossiers, the running example here is **not invented**: the service is specified exactly according to this cut, which is why it serves at once as evidence and as a walk-through from beginning to end. All the values named come from its short specification. What is thereby evidenced is the construction cut; operational experience it is not yet. **And a warning up front:** the concrete names and values here — command designations, token prefix, configuration format, the worker strand for the queue — are *illustration of this one service*, not a house convention. Binding are the procedure in the concept and the rules; whoever copies this section instead of reading it replicates apparatus that their own function may not need at all.

### The draft-filing service, from beginning to end

**One function (F1).** The service puts a mail into an IMAP folder — that is all. The main method receives the message (`POST /messages`, answer `202` with a submission identifier); the helper methods are the file upload for attachments and HTML bodies (`POST /files`, then `GET`/`DELETE` on the file), the status enquiry about the submission (`GET /messages/{id}`) and a readiness point (`GET /healthz`). All share the same access. Here too, what the service does *not* do is remarkable: it holds nothing, searches nothing, offers no retrieval of filed mails — keeping and findability are provided by the mailbox, and every mail client can access it. A storage of its own beside it would be the second source with all the consequences that brings.

**One artifact with modes (F2).** There is exactly one program. `serve` starts the service; `check` checks the configuration and can additionally test the connection to the target system with `--live`; `status` shows local metrics and queue state; under `config` lie setting up, listing and changing sites and accounts including `rotate-token`; `queue` and `files` allow explicit intervention in the queue and the file pool; plus `version`. The worker strand for message delivery belongs to the same program.

**Scriptable commands (F3).** The exit codes are fixed: 0 all good · 1 runtime error · 2 wrong invocation · 3 configuration invalid · 4 live test failed. Listing commands and `status` offer `--json`.

**One delivery artifact (F4).** What is shipped is a Python package and a container image (`ENTRYPOINT ["imap-service","serve"]`, non-root, readiness check on the health endpoint) together with a `deploy/` directory containing a Dockerfile, a compose file, and proxy and systemd configuration. Commands run against the same image.

**One configuration, one judgment (F5).** The configuration is a TOML file; it is looked for in the order `--config-file` → `$IMAP_SERVICE_CONFIG` → user-level location → `/etc/imap-service/config.toml`, and the commands show which file they use. Reading is done with the standard library, writing is format-preserving, validation gives field-precise error messages. Mailbox passwords are not in the file but stand there as a reference (`env:VAR` or `file:/path`).

**State beside it (F6).** One data directory per site and access, with separate areas for uploaded files and queue state. A change of version swaps the image; the store remains.

**No management surface (F7).** The network surface knows no management path. The only unauthenticated point is `GET /healthz`, and it answers exclusively with `{"status":"ok"}` or 503 — no metrics, no internals. It is thus exactly the operational endpoint that F7 permits, and a usable touchstone for the open question Q3.

**The permission cut (F8).** The service runs as a separate, unprivileged user; the systemd unit sets `ProtectSystem=strict` and assigns the configuration and state directories separately. Writing to the configuration happens only in command mode.

**Configuration and access in one act (F9).** `config add-account` is the core: it configures the function for this use case — site, IMAP account, target folder — and creates the token for it, in one motion. The token embodies exactly this triple; callers never choose the account or the folder, neither of which is in their request. It is output exactly once (`imsvc_` plus 24 random characters); persisted are only its SHA-256 and a harmless short identifier (`tk_` plus six characters). `rotate-token` replaces it atomically.

**No administrative rights (F10).** There is no administration token and no administrative user in the service; the three roles of the specification are called operator, client/agent and user — an administrator does not occur. Every access is filtered on *(site, access)*, and foreign as well as nonexistent resources answer identically with 404.

**Outer edge (F11).** The service speaks HTTP exclusively and listens by default on `127.0.0.1:8025`; TLS is terminated by the proxy in front, which serves as the only published entry point. Container operation and a hardened systemd unit are provided for as the process form — the same artifact in both cases.

**State of play, honestly.** The specification is complete, the implementation by a development agent is running; a completion report was still outstanding when this version was written and will be added here. What is thereby evidenced is that this cut can be specified out completely and without contradiction for a real service — not how it holds up over years in operation. In particular the costs named in the scope that accrue only with time — maintenance of the command code, ergonomics, the stiffness from F9 — have not yet materialized and are therefore not refuted either.

### Origin

The cut is not an invention of this dossier. The twelve-factor collection has long demanded that admin processes be run in an environment identical to the service's and that their code be shipped with the application code (Source 2). This dossier draws the sharper consequence from that — not "shipped along with" but "the same artifact in a different mode" — and connects it with two things that do not appear there: the one function (F1) and the creation of the access in the configuration step (F9), which together dissolve the permission topic before it arises.
