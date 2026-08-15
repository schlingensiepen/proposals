# Changelog — capability-tokens-for-agents

<!-- Neueste zuerst. Jede inhaltliche Änderung an PROPOSAL.de.md, jeder Statuswechsel
     (Entwurf → Diskussion → Bewährt → Überholt) und jeder Sync-Vermerk erscheint hier.
     Nichts wird rückwirkend umgedeutet. -->

## 1.0 · 2026-08-14 — Erste vollständige Fassung (Status: Entwurf)

- Vollständige Neufassung auf der Konzept-Vorlage der Reihe, nach Inhaltsdiskussion mit
  dem Autor: Das Dossier ist die **Verallgemeinerung** des Schwester-Konzepts
  `self-describing-services` — es gilt für jeden Agenten-konsumierten Dienst; das
  Schwester-Konzept wird als ausgeführtes Anwendungsbeispiel geführt (§8, Quelle 1).
- Zwölf Festlegungen in drei Blöcken (Zuschnitt F1–F4 · Lebenszyklus F5–F9 ·
  Agenten-Verhalten und Zustellung F10–F12); Selbstregistrierung mit Display-String als
  ausdrücklich **empfohlene** Lösung des Setup-Problems (F5, F11); das Tragwerk zerfällt
  bewusst in zwei tragfähige Hälften (Zuschnitt und Handhabung).
- §9 nach der Lesart des Autors zweigeteilt: Q1–Q3 als **Aufträge an den Verwender**
  (Verfallswerte, Form des zweiten Kanals, Rotations-Rhythmus), Q4–Q7 als echte offene
  Fragen (Präfix-Namensraum, Attenuation/Macaroons, Alt-Token-Meldung,
  Bestandsverwaltungs-Werkzeug — Letzteres als Kandidat `cli-owned-token-lifecycle`
  ausdrücklich außen vor gelassen).
- Aus dem zurückgezogenen Entwurf vom 12.08. übernommen: die Problem-Analyse (§1), die
  Entwurfsregeln (heute F2–F4: Entropie, Präfix, Hash-Speicherung, Einmal-Anzeige,
  Bearer-Header, Orakel-freie Fehlersemantik), die Sicherheitstabelle (§5, erweitert)
  und die klassischen Quellen (Dennis/Van Horn, Hardy, Macaroons; Fundstellen am
  14.08.2026 geprüft, ACM-DOI hinter Verlagsschranke vermerkt).
- Offen: Vollzugsmeldung der Referenzimplementierung (§8 Beleg 3) wird nachgetragen.

## 0.1 · 2026-08-12 — Zurückgezogener Erstentwurf

- Vor der Inhaltsdiskussion und vor der Vier-Felder-Vorlage geschrieben; aus dem Repo
  zurückgezogen und als Steinbruch geführt. Kernbestand (Problem-Analyse, Doppelzuschnitt-
  These, Entwurfsregeln) ist in Fassung 1.0 aufgegangen.
