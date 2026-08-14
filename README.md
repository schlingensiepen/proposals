# Proposals

Eine Reihe eigenständiger Dossiers, die bewährte Konzepte zur Diskussion stellen — und die man
einem KI-Agenten unverändert als Arbeitsanweisung geben kann.

A series of self-contained dossiers that put proven concepts up for discussion — and that can be
handed to an AI agent, unchanged, as a work instruction.

**Agenten-Einstieg / Agent entry:** [AGENTS.md](AGENTS.md) — verzweigt auf zwei Aufgaben /
routes to two jobs: *Konzept oder Kompendium erstellen/pflegen* · *finden und lesen, um zu
verwenden*.

## Register

**[`self-describing-services`](self-describing-services/)** · Konzept / Concept · Entwurf / Draft ·
v1.0 (2026-08-14) — Ein Dienst, der sich selbst beschreibt, macht Installation überflüssig:
Onboarding per URL statt Connector. / A service that describes itself makes installation
unnecessary: onboarding via URL instead of a connector.

**[`headless-imap-provider-access`](headless-imap-provider-access/)** · Kompendium / Compendium ·
Entwurf / Draft · v1.0 (2026-08-14, surveyed 2026-08) — Headless IMAP-Zugang Provider für
Provider: App-Passwörter als Regelfall, Microsoft und Google Workspace als Ausnahmen. /
Headless IMAP access provider by provider: app passwords as the rule, Microsoft and Google
Workspace as the exceptions.

**[`microsoft-imap-app-only`](microsoft-imap-app-only/)** · Kompendium / Compendium · Entwurf /
Draft · v1.0 (2026-08-14, surveyed 2026-08) — IMAP an Microsoft-Postfächern ohne Browser:
Client-Credentials für M365, Device-Code für Privatkonten. / IMAP on Microsoft mailboxes
without a browser: client credentials for M365, device code for personal accounts.

**Geplant / Planned:** `capability-tokens-for-agents` — Kandidaten / candidates:
`maildir-style-file-queues` · `teaching-error-messages` · `self-contained-repos` ·
`two-repo-submodule-pattern` · `cli-owned-token-lifecycle`.

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
