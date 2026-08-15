# Changelog — decision-records-with-madr

<!-- Neueste zuerst. Jede inhaltliche Änderung an PROPOSAL.de.md, jeder Statuswechsel
     (Entwurf → Diskussion → Bewährt → Überholt) und jeder Sync-Vermerk erscheint hier.
     Nichts wird rückwirkend umgedeutet. -->

## 1.0 · 2026-08-13 — Erstfassung (Status: Entwurf)

- Zehn Festlegungen: MADR 4.0.0 unverändert (F1), als einzige Erweiterung beide
  Ersetzungsrichtungen als Listen im Kopf (F2), Kopf ist Buchhaltung und nie Inhalt (F3),
  Dateiname aus vierstelliger Nummer und sprechendem Titel, Nummern nie wiederverwendet
  (F4), fünf Pflichtabschnitte in fester Reihenfolge (F5), Optionen erscheinen zweimal —
  als Liste und als ausgeschriebene Begründung (F6), Kopfschlüssel englisch bei
  einheitlicher Textsprache (F7), eine Vorlage im Bestand, die kopiert wird (F8), der
  Ablösungsvermerk am Anfang des abgelösten Dokuments (F9), das Verzeichnis als Tabelle
  mit Geltungsstand (F10).
- **Eigenständigkeit nachgezogen.** Der erste Wurf benutzte die Unveränderlichkeit
  angenommener Dokumente an sechs Stellen, ohne sie je auszusprechen — ein Leser, der nur
  dieses Dossier hat, hätte F3 nicht verstanden. Die Regel steht jetzt vollständig in den
  Begriffen („Angenommen") und in F3, mit Begründung. Ebenso ergänzt: die
  Nummern-Wiederverwendung in F4 sowie F9 und F10, die vorher nur als Kommentar in der
  Vorlage und im Beispiel auftauchten. Die drei Stellen, an denen eine Ersetzung geführt
  wird — beide Köpfe, das abgelöste Dokument selbst, das Verzeichnis —, sind damit
  vollständig aus diesem Dossier heraus beschrieben.
- **Anlage:** die kopierfertige Vorlage in `examples/`, deutsch und englisch. Ihr Wortlaut
  steht zusätzlich vollständig in Abschnitt 8 beider Sprachfassungen — das Dossier bleibt
  damit auch ohne die Anlage benutzbar.
- Sicherheitsbetrachtung mit vier Gefahren; die erste ist die, an die beim Format niemand
  denkt: Die MADR-Felder `decision-makers`/`consulted`/`informed` laden zu Klarnamen ein,
  und in einem unveränderlichen Bestand bleibt der Name samt Verantwortungszuschreibung
  dauerhaft stehen.
- Bewährung: Bestand von 37 Dokumenten in einem privaten Projekt des Autors. Der Umzug aus
  einer Sammeldatei hat zwei Festlegungen erzwungen — F2, weil drei Dokumente je zwei
  Vorgänger zusammenfassten und ein Einzelfeld einen davon hätte verschweigen müssen, und
  F8, weil die ersten von Hand nachgebauten Dokumente je ein anderes Kopffeld vermissen
  ließen. Status bleibt **Entwurf**.
- Vier offene Fragen: Frage-Feld im Kopf trotz F3 (Q1), kleinster denkbarer Prüflauf für
  die Beziehungslisten (Q2), ob vier Stellen in der Nummer übertrieben sind (Q3),
  mehrsprachige Teams (Q4).
- **Vermerk statt Nacherfindung** (Abschnitt 2 und 10): Die hier beschriebenen Dokumente
  werden einem Mechanismus **unterzogen**, der älter ist als das Format und nicht zu ihm
  gehört — Unveränderlichkeit nach der Annahme, Revision als Ersetzung. Das ist der
  eigentliche Zweck der Übung und zugleich die Daseinsberechtigung von
  Entscheidungsdokumenten überhaupt. Das Dossier sagt das jetzt ausdrücklich, statt die
  Regeln stillschweigend als eigene auszugeben. Die Regeln, auf denen die **eigenen**
  Festlegungen ruhen (F3, F9), bleiben ausgeschrieben — Verweise sind laut Reihe Quelle
  und Vertiefung, nie Voraussetzung.
- **Der Querverweis ist jetzt eine Adresse, kein Name mehr** (Abschnitt 10). Er stand als
  blanker Slug in Backticks da — wer folgen wollte, musste raten, wo. Als https-Adresse,
  weil ein relativer Pfad das Dossier-Verzeichnis verlässt und damit die
  Eigenständigkeitsregel bricht.
- `PROPOSAL.en.md` gleichzeitig erstellt, gleiche Struktur, gleiche F- und Q-Nummern
  (sync: v1.0).
