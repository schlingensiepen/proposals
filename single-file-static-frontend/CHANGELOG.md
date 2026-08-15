# Changelog — single-file-static-frontend

<!-- Neueste zuerst. Jede inhaltliche Änderung an PROPOSAL.de.md, jeder Statuswechsel
     (Entwurf → Diskussion → Bewährt → Überholt) und jeder Sync-Vermerk erscheint hier.
     Nichts wird rückwirkend umgedeutet. -->

## 1.0 · 2026-08-15 — Erstfassung (Status: Entwurf)

- Acht Festlegungen: ein Artefakt ohne Build-Schritt (F1), unauthentifizierte Auslieferung
  und deshalb keine Daten im Dokument (F2), kein Anwendungscode im Auslieferungspfad (F3),
  die Adresse als Zustand des Clients statt als Ressource des Servers (F4), dieser Zustand
  im Fragment, damit der Server nicht umgebogen werden muss (F5), mitgelieferte statt
  eingebundener Fremdmittel (F6), Dateien am API-Umschlag vorbei (F7, **Empfehlung**), eine
  Origin für Auslieferung und API (F8, **Empfehlung**).
- **Der Kern des Zuschnitts:** einfaches Deployment unter Einhaltung struktureller
  Sicherheit. Beide Hälften sind dieselbe Sache von zwei Seiten — *weil die Datei jeder
  bekommen darf, darf nichts drinstehen; weil nichts drinsteht, ist das Ausliefern ein
  Kopiervorgang.* Die Hosting-Beschränkung („kein URL-Rewriting") ist in diesem Dossier
  ausdrücklich **ein Beispiel und nicht die Ursache** — anders als im Schwester-Dossier
  zur API, wo sie die Ursache ist.
- **Fragment-Routing steht als Zuschnittfrage da, nicht als Sicherheitsargument.** F4
  benennt die Verschiebung (die Adresse ist ein Client-Zustand), F5 zieht die Konsequenz
  für die Schreibweise; die Tabelle dort trägt den Kern in einer Zeile: *Was ausgeliefert
  wird — Artefakt **und** Serverkonfiguration, gegen: Artefakt.* Dazu der praktische
  Zusatzgewinn: Ändert sich nur das Fragment, entsteht **keine Anfrage**, kein Neuladen,
  derselbe Kontext. Einladungslinks sind ausschließlich ein Verweis in Abschnitt 10.
- **F2 nachgeschärft:** Das Dokument enthält *ausschließlich zweierlei — Form und
  Hydrations-Logik*. Dazu die Prüffrage für Grenzfälle: *Stünde hier bei einem anderen
  Bestand etwas anderes?* Eine verhandelbare Grenze wird verhandelt; diese ist anwendbar.
- Sicherheitsbetrachtung mit sechs Gefahren, darunter die beiden, die dieser Zuschnitt
  geradezu einlädt: **Verstecken statt schützen** (was im Client ausgeblendet ist, steht im
  öffentlichen Artefakt im Klartext) und **alles hängt an der API** — das ist gewollt, aber
  nur tragfähig, wenn dort kein Aufruf sich darauf verlässt, dass „das Frontend das ohnehin
  nicht anbietet".
- **Bewährung: keine Referenz-Implementierung.** Die laufende Anwendung stammt aus der Zeit
  vor dieser Entscheidung und liefert mehrere HTML-Dateien aus. Ausdrücklich vermerkt, dass
  die Aussagen **durchdacht, nicht erprobt** sind. Status bleibt **Entwurf**.
- Vier offene Fragen: Größengrenze von F1 (Q1), Aktualität mitgelieferter Fremdmittel (Q2),
  der leere erste Bildschirm (Q3), tiefe Zustände im Fragment (Q4).
- `PROPOSAL.en.md` und `AGENT.md` gleichzeitig erstellt; gleiche Struktur, gleiche F- und
  Q-Nummern (sync: v1.0).
