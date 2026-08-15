# Changelog — immutable-decision-records

<!-- Neueste zuerst. Jede inhaltliche Änderung an PROPOSAL.de.md, jeder Statuswechsel
     (Entwurf → Diskussion → Bewährt → Überholt) und jeder Sync-Vermerk erscheint hier.
     Nichts wird rückwirkend umgedeutet. -->

## 1.0 · 2026-08-13 — Erstfassung (Status: Entwurf)

- Acht Festlegungen: ein Dokument je Entscheidung (F1), Unveränderlichkeit nach der
  Annahme (F2) — bezogen auf den **Inhalt**, mit **Kopf und Dokumentenanfang
  ausdrücklich ausgenommen**, weil der Rückverweis später entsteht als das Dokument, das
  ihn trägt, und die Beziehung sonst nur vorwärts zu führen wäre —,
  vollständige statt teilweiser Ersetzung (F3), das abgelöste Dokument
  bleibt stehen und behält seinen Titel (F4), Ersetzung als n:m-Beziehung an beiden Enden
  (F5), keine Wiederverwendung von Nummern (F6), verworfene Alternativen als Pflichtteil
  (F7), Verzeichnis über den Geltungsstand (F8).
- Sicherheitsbetrachtung mit vier Gefahren; die erste — ein Geheimnis wird durch die
  Unveränderlichkeit selbst konserviert — ist die einzige, bei der das Konzept gegen sich
  selbst arbeitet: Geheimnisrotation hat Vorrang, die Entfernung aus dem Text ist eine
  ausdrückliche Ausnahme.
- Bewährung: Bestand von 37 Dokumenten in einem privaten Projekt des Autors, entstanden
  aus dem Umzug einer Sammeldatei; zwei Erfahrungen aus diesem Umzug haben F4 und die
  Migrationsregel „im Zweifel behalten" geschärft. Status bleibt **Entwurf** — die
  Kipp-Bedingung „hohe Änderungsfrequenz" wurde noch nicht erlebt.
- Vier offene Fragen: Größengrenze des handgepflegten Verzeichnisses (Q1), sehr lange
  Dokumente mit kleiner Änderung (Q2), Chronik-Dokument als Ausnahme von F1 (Q3),
  Quelltext-Verweise auf abgelöste Nummern (Q4).
- **Verhältnis zu ADR/MADR benannt** (Abschnitt 8): Der Mechanismus ist nicht neu — er ist
  der Kern dessen, was seit 2011 als Architecture Decision Record praktiziert wird. Der
  Beitrag dieses Dossiers ist die **Ablösung des Mechanismus vom Format**: Die Frage
  „wie halte ich Entscheidungen nachvollziehbar?" stellt sich bei Produkt-, Beschaffungs-
  und redaktionellen Entscheidungen genauso, und dort führt das Vokabular
  „Architecture Decision Record" in die Irre. Das ist zugleich die Begründung dafür, dass
  es als eigenes Dossier neben `decision-records-with-madr` steht.
- **Querverweis auf `decision-records-with-madr` ergänzt** (Abschnitt 10). Er fehlte ganz —
  der Verweis war einseitig, und wer dieses Dossier las, erfuhr nicht, dass es die fertige
  Form dazu gibt. Als https-Adresse, weil ein relativer Pfad das Dossier-Verzeichnis
  verlässt und damit die Eigenständigkeitsregel bricht.
- `PROPOSAL.en.md` gleichzeitig erstellt, gleiche Struktur, gleiche F- und Q-Nummern
  (sync: v1.0).
