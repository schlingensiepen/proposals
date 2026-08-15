# Changelog — service-as-single-artifact

<!-- Neueste zuerst. Jede inhaltliche Änderung an PROPOSAL.de.md, jeder Statuswechsel
     (Entwurf → Diskussion → Bewährt → Überholt) und jeder Sync-Vermerk erscheint hier.
     Nichts wird rückwirkend umgedeutet. -->

## 1.0 · 2026-08-15 — Erste vollständige Fassung (Status: Entwurf)

- Erstfassung nach Inhaltsdiskussion mit dem Autor. Das Dossier ist ausdrücklich ein
  **Vorgehen**, kein Entwurfsmuster für Schnittstellen: Adressat ist, wer eine Funktion
  ins Netz stellen will und dafür einen kleinen Dienst baut.
- §2 trägt das Vorgehen in elf Schritten (0 bis 10), je Schritt mit dem Ergebnis, das am
  Ende vorliegt; §4 die elf Festlegungen, die es tragen — Zuschnitt (F1–F6), Trennung
  (F7–F10), Außenkante (F11).
- Tragende Klammer ist F1 („Ein Dienst, eine Funktion"): Haupt- und Hilfsmethoden dienen
  derselben Funktion und teilen den Zugang; mit der zweiten unabhängigen Funktion kehren
  Rechteunterscheidung, Rollen und Verwaltung zurück. Kern des Zuschnitts ist F9
  („Konfiguration und Zugang entstehen in einem Akt"), aus dem F10 folgt: Der Dienst
  kennt keine Verwaltungsrechte, weil es keinen Zeitpunkt gibt, an dem jemand einem
  bestehenden Zugang etwas hinzufügt.
- §8 ist kein erfundenes Beispiel: Der Entwurfsablage-Dienst des Autors ist genau nach
  diesem Zuschnitt spezifiziert und dient zugleich als Beleg und als Durchlauf; alle
  Werte stammen aus seiner Kurzspezifikation. Der Abschnitt warnt ausdrücklich davor,
  die dortigen Namen und Werte als Hauskonvention zu lesen.
- Vor der Veröffentlichung zweimal praktisch geprüft: Ein kontextfreier Agent sollte
  allein aus dem Dossier und einer Anwenderbeschreibung ein anderes Werkzeug bauen. Aus
  dem ersten Durchgang stammen vier Korrekturen — die Geheimnis-Rangfolge steht jetzt
  ausgeschrieben in F5 (das Dossier verwies dafür auf ein Schwester-Dossier und war
  damit nicht selbsttragend); F8 unterscheidet Konfiguration von Betriebsdaten (der
  Dienst muss Letztere schreiben); Schritt 6 und F9 widersprachen sich beim Ausstellen
  des Zugangs; §8 trägt die Illustrationswarnung. Zusätzlich kam Schritt 0 hinzu: die
  Frage, ob es überhaupt ein Dienst sein muss. Der zweite Durchgang bestätigte die
  Selbsttragung ausdrücklich; die verbliebenen offenen Punkte waren fachlicher Natur.
- Nach dem zweiten Durchgang ergänzt: die Begründung in F9, warum der Zugang auch im
  Alleinbetrieb von Anfang an eingebaut gehört (kostet dort nichts, Nachrüsten ist
  teuer) — und die Auslieferungskette in F4 und Schritt 9, samt der Pflicht, den
  Anwender aktiv nach Image, Bauleitung und Ausrolldatei zu fragen.
- Offen: Vollzugsmeldung der Referenzimplementierung (§8) wird nachgetragen.
