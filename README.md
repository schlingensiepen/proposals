# Proposals

Eine Reihe eigenständiger Dossiers, die bewährte Konzepte zur Diskussion stellen — und die man
einem KI-Agenten unverändert als Arbeitsanweisung geben kann.

A series of self-contained dossiers that put proven concepts up for discussion — and that can be
handed to an AI agent, unchanged, as a work instruction.

**Agenten-Einstieg / Agent entry:** [AGENTS.md](AGENTS.md) — verzweigt auf zwei Aufgaben /
routes to two jobs: *Konzept oder Kompendium erstellen/pflegen* · *finden und lesen, um zu
verwenden*.

## Register

> **Migration:** Die Reihe wechselt auf eine neue Form: je Proposal ein englisches
> OKF-Bundle mit `README.md`, `index.md`, `rules/`, `choices/`, `terms.md` (englisch/deutsch)
> und `AGENT.md`. Das erste Bundle in dieser Form ist `one-configuration-one-judgment`; die
> übrigen Dossiers folgen bei ihrer nächsten Revision. Bis dahin beschreiben `AGENTS.md` und
> `_template/` noch die bisherige Form. / The series is moving to a new form: one English OKF
> bundle per proposal with `README.md`, `index.md`, `rules/`, `choices/`, `terms.md`
> (English/German) and `AGENT.md`. The first bundle in this form is
> `one-configuration-one-judgment`; the other dossiers follow at their next revision. Until
> then `AGENTS.md` and `_template/` still describe the previous form.

**[`one-configuration-one-judgment`](one-configuration-one-judgment/)** · Konzept / Concept ·
Entwurf / Draft · v1.0 (2026-10-01) · OKF-Bundle, englisch / OKF bundle, English — Eine
Konfiguration, ein Urteil: geprüft wird mit dem Code, der startet; jeder Wert nennt seine Quelle;
leer oder untauglich ist ein gemeldeter Fehler, keine stille Vorgabe. Importierbares Grundkonzept.
/ One configuration, judged once by the code that will run on it; every value names where it came
from, and an empty or unusable value is a reported error, never a silent default. An importable
base concept.

**[`capability-tokens-for-agents`](capability-tokens-for-agents/)** · Konzept / Concept · Entwurf /
Draft · v1.0 (2026-08-14) — Ein Token verkörpert genau eine Fähigkeit an genau einem Ort, und seine
Handhabung folgt festen Regeln entlang des Lebenszyklus; das allgemeine Konzept für
Agenten-konsumierte Dienste. / A token embodies exactly one capability at exactly one place, and its
handling follows fixed rules along the lifecycle; the general concept for agent-consumed services.

**[`self-describing-services`](self-describing-services/)** · Konzept / Concept · Entwurf / Draft ·
v1.0 (2026-08-14) — Ein Dienst, der sich selbst beschreibt, macht Installation überflüssig:
Onboarding per URL statt Connector. Entwurfsmuster für die Schnittstelle; ausgeführte Anwendung von
`capability-tokens-for-agents`. / A service that describes itself makes installation unnecessary:
onboarding via URL instead of a connector. A design pattern for the interface, and a worked
application of `capability-tokens-for-agents`.

**[`service-as-single-artifact`](service-as-single-artifact/)** · Konzept / Concept · Entwurf /
Draft · v1.0 (2026-08-15) — Ein Vorgehen in elf Schritten für den, der eine Funktion ins Netz
stellen will: ein Dienst, eine Funktion, ein Artefakt, ein Erzeugnis — und ein Zugang, der mit
seiner Konfiguration entsteht. Kein Entwurfsmuster für Schnittstellen, sondern der Bauweg dorthin. /
An eleven-step procedure for whoever wants to put one function on the network: one service, one
function, one artifact, one delivery artifact — and an access that arises together with its
configuration. Not an interface design pattern but the way to build one.

**[`headless-imap-provider-access`](headless-imap-provider-access/)** · Kompendium / Compendium ·
Entwurf / Draft · v1.0 (2026-08-14, surveyed 2026-08) — Headless IMAP-Zugang Provider für
Provider: App-Passwörter als Regelfall, Microsoft und Google Workspace als Ausnahmen. /
Headless IMAP access provider by provider: app passwords as the rule, Microsoft and Google
Workspace as the exceptions.

**[`microsoft-imap-app-only`](microsoft-imap-app-only/)** · Kompendium / Compendium · Entwurf /
Draft · v1.0 (2026-08-14, surveyed 2026-08) — IMAP an Microsoft-Postfächern ohne Browser:
Client-Credentials für M365, Device-Code für Privatkonten. / IMAP on Microsoft mailboxes
without a browser: client credentials for M365, device code for personal accounts.

**[`immutable-decision-records`](immutable-decision-records/)** · Konzept / Concept · Entwurf /
Draft · v1.0 (2026-08-13) — Ein angenommenes Entscheidungsdokument wird nie mehr geändert; jede
Änderung ist ein neues, vollständiges Dokument, das das alte ersetzt. Der Mechanismus,
formunabhängig. / An accepted decision record is never changed again; every change is a new,
complete record replacing the old one. The mechanism, independent of any format.

**[`decision-records-with-madr`](decision-records-with-madr/)** · Konzept / Concept · Entwurf /
Draft · v1.0 (2026-08-13) — MADR unverändert übernommen und an genau einer Stelle erweitert: die
Ersetzung steht als zwei Listen im Kopf. Mit kopierfertiger Vorlage; die Form zum Mechanismus von
`immutable-decision-records`. / MADR adopted unchanged and extended in exactly one place:
replacement as two lists in the head. Ships a template; the form to the mechanism of
`immutable-decision-records`.

**[`single-endpoint-json-rpc`](single-endpoint-json-rpc/)** · Konzept / Concept · Entwurf / Draft ·
v1.0 (2026-08-14) — Die gesamte API als ein POST-Endpunkt mit JSON-RPC-2.0-Umschlag; Nachweise in
Kopfzeilen, Binärdaten außerhalb des Umschlags, große Zusammenstellungen beauftragt statt abgelegt.
/ The entire API as one POST endpoint with a JSON-RPC 2.0 envelope; credentials in headers, binary
data outside the envelope, large compilations commissioned rather than stored.

**[`single-file-static-frontend`](single-file-static-frontend/)** · Konzept / Concept · Entwurf /
Draft · v1.0 (2026-08-15) — Das Frontend als ein statisches Artefakt ohne Bauschritt: Weil die
Datei jeder bekommen darf, darf nichts drinstehen — und weil nichts drinsteht, ist das Ausliefern
ein Kopiervorgang. / The frontend as one static artefact with no build step: because anyone may
have the file, nothing may be in it — and because nothing is in it, deploying is a copy operation.

**[`invitation-instead-of-login`](invitation-instead-of-login/)** · Konzept / Concept · Entwurf /
Draft · v1.0 (2026-08-15) — Zugang entsteht durch Einladung: Ein Link lädt einen Benutzer für alle
seine Geräte ein, weil jedes Einlösen einen eigenen Nachweis erzeugt; Maschinen bekommen Fähigkeit
statt Identität. / Access arises from an invitation: one link invites a user for all their devices,
because every redemption creates its own credential; machines receive capability rather than
identity.

**Kandidaten / Candidates:** `maildir-style-file-queues` · `self-contained-repos` ·
`two-repo-submodule-pattern` · `teaching-error-messages` (Prüfung offen, ob neben
`self-describing-services` F7 eigenständig tragfähig / pending review whether it stands on its own
beside `self-describing-services` F7).

## Aufbau / Layout

Ein Verzeichnis = ein Dossier; sprechender englischer Slug, keine Nummern. Je Dossier:
**`PROPOSAL.de.md`** (Diskussionspapier; deutsches Original, führend) · **`PROPOSAL.en.md`**
(englische Fassung; Kopfvermerk *„Translated from the German original · sync: vX.Y"* — Rückstand ist
erlaubt, aber nie unsichtbar: dann *„sync: pending (de vX.Y)"*) · **`AGENT.md`** (Arbeitsanweisung
für Agenten; englisch, Skill-Format: als Skill laden oder unverändert ins Projekt legen) ·
`README.md` · `CHANGELOG.md` · optional `examples/` (Code englisch, wie aller Code).

One directory = one dossier; descriptive English slug, no numbers. Per dossier: **`PROPOSAL.de.md`**
(discussion paper; German original, leading) · **`PROPOSAL.en.md`** (English version; header note
*"Translated from the German original · sync: vX.Y"* — lagging is allowed but never invisible:
*"sync: pending (de vX.Y)"*) · **`AGENT.md`** (work instruction for agents; English, skill format:
load as a skill or drop into the project unchanged) · `README.md` · `CHANGELOG.md` · optional
`examples/`.

**Vorlagen / Templates:** [`_template/konzept/`](_template/konzept/) und
[`_template/kompendium/`](_template/kompendium/) — je ein vollständiges, kopierbares
Dossier-Verzeichnis mit eingebetteten Regeln: Frontmatter-Kopf mit `question`/`thesis` (die
ersten Zeilen entscheiden, ob ein Agent das ganze Dokument lädt), Festlegungsblöcke F1… mit vier
Feldern (Was gilt · Warum · Naheliegend und verworfen · Symptom der Verletzung), „entfällt,
weil …", Verweise nur im Quellen-Abschnitt; `AGENT.md` ist selbsttragend — F-Nummern dort nur
als Herkunftsvermerk. / Two complete, copyable dossier directories with embedded rules:
frontmatter head with `question`/`thesis` (the first lines decide whether an agent loads the
whole document), decision blocks F1… with four fields (what holds · why · obvious and rejected ·
symptom of violation), "omitted, because …", references only in the sources section; `AGENT.md`
is self-contained — F numbers there are provenance markers only.

## Typen & Status / Types & status

**Typen:** `Konzept` (These + Festlegungen) · `Kompendium` (kuratiertes Fakten-/Betriebswissen;
statt „Offene Fragen" ein datierter „Stand der Recherche") · `Implementierungskonzept` (in sich
geschlossene Beschreibung eines zu bauenden Systems; nutzt die Konzept-Vorlage mit drei
Umdeutungen: „Festlegungen" werden normativ — „Was gilt" als MUSS/SOLL/KANN-Satz, Fn als
Anforderungs-Kennung —, „Geltung und Grenzen" nennt die Nicht-Ziele, „Bewährung" wird „Stand der
Umsetzung". Das Dossier trägt Spezifikation und Begründung in sich — nichts wird ausgelagert). /
**Types:** `Concept` (thesis + decisions) · `Compendium` (curated factual knowledge; a dated
"state of research" instead of open questions) · `Implementation concept` (a self-contained
description of a system to be built; uses the Konzept template with three reinterpretations:
decisions become normative — "what holds" as a MUST/SHOULD/MAY sentence, Fn as the requirement
identifier —, "scope and limits" names the non-goals, "track record" becomes "implementation
status". The dossier carries spec and rationale itself — nothing is externalized).

**Status:** Entwurf/Draft → Diskussion/Discussion → Bewährt/Proven (nur mit Referenznachweis im
Abschnitt „Bewährung" / only with reference evidence in the track-record section) → ggf. Überholt/Superseded (mit
Nachfolger-Verweis / with a successor reference). Wechsel ausschließlich über `CHANGELOG.md`, nie
rückwirkend. / Transitions only via `CHANGELOG.md`, never retroactively.

## Eigenständigkeit / Self-containment

Jedes Dossier ist ohne die anderen verständlich und anwendbar — in jeder Sprachfassung. Querverweise
auf Schwester-Dossiers sind Quellen/Vertiefung, nie Voraussetzung; nötiges Wissen wird hineingeholt,
auch um den Preis von Redundanz. Prüfregel: *Versteht und befolgt man das Dossier, wenn alle Links
tot sind?* / Each dossier is understandable and applicable without the others — in every language
version. Cross-references are sources, never prerequisites; required knowledge is pulled in, even at
the price of redundancy. Test: *does the dossier still work when every link is dead?*

## Diskussion / Discussion

Einwände und Beiträge je Dossier über Issues/Discussions, adressiert über die Q-Nummern der „Offenen
Fragen"; angenommene Einwände werden neue Version + CHANGELOG-Eintrag. / Objections and contributions
per dossier via issues/discussions, addressed through the Q numbers of the "open questions"; accepted
objections become a new version plus a changelog entry.

## Lizenz / License

[CC BY 4.0](LICENSE) — Namensnennung / attribution: „der Autor" dieses Repositoriums / "the author"
of this repository.
