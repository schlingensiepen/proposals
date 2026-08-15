# Changelog — invitation-instead-of-login

<!-- Neueste zuerst. Jede inhaltliche Änderung an PROPOSAL.de.md, jeder Statuswechsel
     (Entwurf → Diskussion → Bewährt → Überholt) und jeder Sync-Vermerk erscheint hier.
     Nichts wird rückwirkend umgedeutet. -->

## 1.0 · 2026-08-15 — Erstfassung (Status: Entwurf)

- Zehn Festlegungen: keine Gäste (F1), Einladung statt Registrierung (F2), Einladungstoken
  ist nicht der Nachweis und jedes Gerät kennt seine Herkunft (F3), ein Nachweis je Gerät —
  „Benutzer auf Gerät" (F4), Autorisierung gegen den Bestand statt gegen den Nachweis (F5),
  Identität für Menschen und Fähigkeit für Maschinen (F6), Maschinenzugänge nur im
  Bootstrap-Fenster (F7), Bootstrap als Zeitfenster statt Löschdatei (F8), Geheimnisse in
  Adressen nur im Fragment (F9), zwei Anker für ein Rechtesystem samt fünf Anforderungen
  daran (F10).
- **Ein Link, alle Geräte — als Vorteil, nicht als Zugeständnis.** Die Erstfassung
  formulierte den Tausch an fünf Stellen als „einmal" und stellte damit die Kernthese gegen
  sich selbst: Der Link ist **beliebig oft** einlösbar, je Gerät genau einmal. Richtiggestellt
  und in F3 zum Zweck erklärt — die Einladung ist eine Aussage über die **Person**, nicht
  über ein Endgerät.
- **Wo der Nachweis liegt** (F4): in einem Cookie, das der **Server** setzt — mit beiden
  Gründen. Der bekannte (`HttpOnly`, der Client kann es nicht lesen) und der meist
  vergessene: Skriptgesetzter Speicher wird vom Tracking-Schutz nach wenigen Tagen ohne
  Nutzung geräumt. *Wer den Nachweis dort ablegt, baut ein Modell, das nach einer Woche
  Urlaub jeden aussperrt.* Dazu die 400-Tage-Obergrenze und die Falle, dass ein Aliasname
  auf einen fremden Host auch server-gesetzte Cookies kappt.
- **„Gerät" heißt in Wahrheit „Browserprofil"** (F4): Zwei Browser auf einem Rechner ergeben
  zwei Einträge für ein Stück Blech. Kein Fehler des Modells — es zählt Zugänge, nicht
  Hardware —, aber überraschend. Daraus die Empfehlung, den vollständigen Kennungsstring des
  Browsers und die zuletzt gesehene Netzadresse aufzuheben, nur den letzten Wert: als
  **Wiedererkennungshilfe für den Menschen vor der Liste**, nicht als Kennung.
- **Drei Hebel gegen den weitergegebenen Link**, die einander nicht ersetzen (F3): Ablauf
  (wirkt vorbeugend, ohne Aufmerksamkeit), Zurückziehen (beendet vorzeitig), Entziehen
  einzelner Geräte (räumt Entstandenes auf). Der Ablauf erzwingt den zweiten
  Einladungsvorgang aus F2 — sonst entstehen Dubletten der Person.
- **Erzeugen ist befristet, Entziehen ist jederzeit** (F7). Die Asymmetrie ist Absicht: Ein
  Vorgang, der Rechte wegnimmt, darf nie schwerer erreichbar sein als der, der sie gibt.
- **F10 formuliert Anforderungen an ein Rechtesystem, kein Rechtemodell.** Der Zuschnitt der
  Fachrechte ist offen, die Zuständigkeit für die Vorgänge des Modells ist es nicht.
- Abgrenzung gegen `capability-tokens-for-agents` am tatsächlichen Text gezogen (Abschnitt
  10): dort die Fähigkeitsseite allein, mit einem Agenten, der sich selbst anmeldet; hier
  die Grenze zwischen beiden Arten in einem System, das beide führt. *Ein Agent taucht auf,
  ein Hintergrundprozess wird hingestellt.*
- **Bewährung: keine Referenz-Implementierung.** Die laufende Anwendung stammt aus der Zeit
  mit anonymer Lesestufe — dem Zustand, den F1 ausschließt. Status bleibt **Entwurf**.
- `PROPOSAL.en.md` und `AGENT.md` gleichzeitig erstellt; gleiche Struktur, gleiche F- und
  Q-Nummern (sync: v1.0).
