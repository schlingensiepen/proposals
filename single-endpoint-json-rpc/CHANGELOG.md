# Changelog — single-endpoint-json-rpc

<!-- Neueste zuerst. Jede inhaltliche Änderung an PROPOSAL.de.md, jeder Statuswechsel
     (Entwurf → Diskussion → Bewährt → Überholt) und jeder Sync-Vermerk erscheint hier.
     Nichts wird rückwirkend umgedeutet. -->

## 1.0 · 2026-08-14 — Erstfassung (Status: Entwurf)

- Zehn Festlegungen: eine Adresse über das Standarddokument (F1), JSON-RPC 2.0 als
  etablierter Umschlag (F2), kein Batch und keine Notifications (F3), HTTP-Status stets
  `200` mit genau zwei begründeten Ausnahmen (F4), Fehler mit drei Feldern für drei
  Verbraucher (F5), getrennte Nummernräume für Protokoll- und Fachfehler (F6), Nachweise
  ausschließlich in Headern (F7), Binärdaten außerhalb des Umschlags in beide Richtungen
  (F8), Dateiannahme ohne Fachlogik (F9), eine Origin für statische Auslieferung und API
  (F10).
- **Tragwerk:** F1 trägt alles — fällt die Randbedingung „kein URL-Rewriting", ist auch
  der Hauptgrund für F2 hinfällig und REST wird eine ernsthafte Alternative. F8 und F9
  sind ausdrücklich der **Preis** von F2, nicht seine Ergänzung. F7 ist die Festlegung mit
  der größten Reichweite und einzeln übernehmbar.
- **Sicherheitsbetrachtung** mit sechs Gefahren. Zwei davon sind unbequem und stehen
  deshalb ausgeschrieben da: Die unerratbare Datei-Adresse ist ein Nachweis ohne Rücknahme
  und ohne Ablauf — das ist eine Grenze der Plattform, keine Nachlässigkeit. Und ein
  einziger Endpunkt macht jeden Angriff im Zugriffslog ununterscheidbar vom Normalbetrieb;
  wer Erkennung braucht, muss sie in der Anwendung führen.
- **Kosten** ausgeschrieben, darunter der unangenehmste Punkt: Aus „einem Endpunkt" sind
  faktisch **drei Eingänge** geworden — API, Upload, Binärabruf. F8 und F9 sind kein
  Detail, sondern ein Drittel der Angriffsfläche.
- **Bewährung: keine Referenz-Implementierung.** Das Konzept ist vollständig spezifiziert,
  die laufende Anwendung bildet es aber noch nicht ab. Ausdrücklich vermerkt, dass die
  Aussagen **durchdacht, nicht erprobt** sind — insbesondere die Atomarität aus F9 ist
  bisher nur ein Argument. Status bleibt **Entwurf**.
- Vier offene Fragen: Versionierung bei einem Endpunkt (Q1), Verhalten bei
  vorgeschaltetem Zwischenserver (Q2), Grenze für Klartext bei internen Fehlern (Q3),
  unerratbare Adressen ohne Ablauf (Q4).
- **Verhältnis zu `single-file-static-frontend`** in Abschnitt 10 benannt: Die beiden
  Dossiers überschneiden sich an zwei Stellen — Ausnahme für Dateien (F8, F9) und eine
  Origin (F10). Hier sind es **Vorgaben** (konkreter Stack), dort **Empfehlungen**
  (Entwurfsmuster). Die Doppelung ist bewusst; ändert sich eines der beiden Themen, ist
  **beides** anzufassen.
- **Nachbesserung nach Erstlesung (14.08.) — vier Befunde, alle berechtigt:**
  - **F7 war unzureichend.** „Nachweise stehen im Header" sagte nicht, *welche* Arten es
    gibt, wer den Sitzungsnachweis setzt und woher er kommt. Jetzt mit Tabelle der drei
    Arten, dem Tausch gegen einen Einlöse-Nachweis und — der eigentliche Gewinn — dem
    **Fragment** als einziger Stelle einer Adresse, an der ein Geheimnis stehen darf, weil
    der Browser es nicht an den Server schickt. Damit ist F7 in sich geschlossen; das
    Zugriffsmodell selbst bleibt ausdrücklich draußen.
  - **F8 verschwieg, dass sein Einwand auch für Tokens gilt.** „Inhaber-Nachweis ohne
    Rücknahme" trifft auf jeden Token zu. Der Unterschied liegt **nicht in der Natur,
    sondern im Ort**: Ein Pfad wird protokolliert, ein Header nicht. Steht jetzt so da,
    samt der Bedingung, unter der es hinnehmbar ist — der Verlust muss **einzeln** bleiben.
  - **F9 setzte einen Mechanismus voraus, der nirgends beschrieben war.** Ein
    beschreibbarer Bereich, den der Webserver gewöhnlich ausliefert; ein zufällig benanntes
    Unterverzeichnis je Datei; die Datei behält ihren Upload-Namen. Das ist jetzt **F11**,
    mit der Begründung, warum der Zufall ins Verzeichnis gehört und nicht in den
    Dateinamen: Ohne Anwendungscode im Auslieferungspfad gibt es keinen
    `Content-Disposition`-Header, also muss der Dateiname sprechend bleiben. Die Bauweise
    folgt damit aus F8.
  - **Der zweite Riegel in F9 war zu Unrecht Pflicht.** Das Abschalten der
    Skriptausführung braucht eine Serverkonfiguration — genau das, was auf günstigem
    Hosting nicht vorausgesetzt werden darf. Die Riegel sind jetzt **nach Verbindlichkeit
    gestuft**, und der zuvor **ganz fehlende** kam dazu: `index.html` **und** `index.php`
    in jedem ausgelieferten Verzeichnis, weil das ohne jede Serverkonfiguration
    funktioniert und dieselbe Standarddokument-Mechanik nutzt wie F1. Ohne ihn ist F8
    wertlos — ein Listing macht aus „unerratbar" schlicht „aufzählbar".
- **F12 neu, als allgemeines Muster statt als Verbot:** Muss die
  Anwendung größere Bestände zusammenstellen, gibt es Methoden im Endpunkt —
  **beauftragen**, bei nebenläufiger Erstellung **abfragen** (die Adresse entsteht erst
  nach Abschluss), abholen, **beenden**. Die Beendigung ist bei sicherheitsrelevanten
  Daten Pflicht; für kritische Daten zusätzlich eine Frist, **träge vollstreckt** durch
  jeden Aufruf dieser Methoden, sodass kein zeitgesteuerter Prozess nötig ist. Der Umschlag
  trägt die Bytes nicht, aber er trägt den **Auftrag**. Am Sicherungsmechanismus in
  Abschnitt 8 vollständig durchdekliniert.
- **Kosten korrigiert:** Aus „drei Eingängen" sind vier geworden — API, Upload,
  Binärabruf, Bereitstellung. Fünf der zwölf Festlegungen behandeln die Ausnahmen.
- Zwei neue offene Fragen: die Lücke der trägen Vollstreckung (Q4, neu gefasst) und die
  ungeprüfte Annahme, dass `index.html` das Listing **überall** unterdrückt (Q5).
- **F7 ist ein Auftrag an den Bauer, keine Auslassung** (Nachtrag 14.08.): Die
  Frage „woher kommt das Cookie?" stand zunächst unter „Nicht gelöst". Falsch — wer eine
  Anwendung nach diesem Muster baut, **muss** fünf Mechanismen bereitstellen: Einladung
  erzeugen, gegen einen Sitzungsnachweis tauschen, bei jedem Besuch erneuern, einzeln
  entziehen (was voraussetzt, dass die Nachweise unterscheidbar sind), Maschinenschlüssel
  genau einmal im Klartext ausgeben. Ohne sie ist F7 nicht umsetzbar. Abschnitt 7 trennt
  das jetzt ausdrücklich: **Mechanismen sind Pflicht, die Politik ist offen** — wer
  einladen darf, wie lange, wie viele Geräte, ob es Rollen gibt.
- **Alles nachgezogen (14.08.):** `PROPOSAL.en.md` ist wieder deckungsgleich — F1–F12,
  Q1–Q5, zehn Abschnitte, `sync: v0.1`. `AGENT.md` trägt die fünf Mechanismen als eigene
  Regel samt zwei Prüffragen und drei neuen Fehlerzeilen. Der Zwischenstand
  `sync: pending` bestand knapp einen Tag und ist damit erledigt.
