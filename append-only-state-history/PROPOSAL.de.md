---
type: Konzept
status: Entwurf
version: "0.2 · Fragment — die Frage der Relationen ist ungeklärt"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: append-only-state-history
language: de (führend)
question: "Wie halte ich die Geschichte eines Gegenstands fest, ohne dass sie sich nachträglich ändern lässt — und ohne dass jede Änderung alles verdoppelt?"
thesis: "Nichts wird überschrieben: Jede Änderung erzeugt einen neuen Zustand, Anhänge werden per Verweis übernommen statt kopiert, und genau ein Metadatum darf am jeweils letzten Zustand noch leben."
---

> # ⚠ Fragment — bewusst unfertig
>
> **Dieses Dossier ist eine Mahnung, kein Konzept.** Es hält fest, was
> entschieden ist, damit es nicht verlorengeht — und benennt die eine Frage, an
> der es hängt: **wie sich das Verfahren auf Objektbäume überträgt.**
>
> Solange die ungeklärt ist, wird hier nicht auf 1.0 gehoben. Die Festlegungen
> unten sind an einem **flachen** Modell entstanden (ein Gegenstand, eine Folge
> von Zuständen, Anhänge daran) und dort erprobt; ob sie tragen, wenn die Sache
> selbst eine Baumstruktur hat, ist offen und in Abschnitt 9 als Q1
> ausgeschrieben. Der Autor hat dazu Überlegungen, die noch nicht eingeflossen
> sind.
>
> **Wer dieses Fragment weiterverwendet, muss wissen:** Für flache Gegenstände
> ist es anwendbar. Für Bäume ist es **keine Antwort**, sondern eine offene
> Frage mit einer Vorarbeit.

# Nichts wird überschrieben

**In einem Satz:** Der Zustand eines Gegenstands wird nie geändert, sondern
ersetzt — jede Änderung erzeugt einen neuen, vollständigen Zustand, und die
Geschichte ist die Folge dieser Zustände.

**Kurzthese:** Wo Nachvollziehbarkeit zählt, ist ein änderbarer Datensatz das
Grundproblem: Man sieht am Ende nur den letzten Stand und weiß nicht, was wann
galt. Eine unveränderliche Zustandsfolge stellt beides her — zum Preis, dass
Anhänge nicht kopiert, sondern übernommen werden müssen, und dass es genau eine
begründete Ausnahme gibt.

## 1. Problem

„Ich fotografiere den Inhalt einer Kiste, jemand ergänzt später eine
Beschreibung, ein halbes Jahr danach fehlt ein Gegenstand. Wenn ich den
Datensatz jedes Mal überschreibe, ist am Ende nur der letzte Stand da — und die
Frage ‚wann ist das verschwunden?' ist nicht mehr zu beantworten. Wenn ich
dagegen jede Änderung als vollständige Kopie ablege, verdoppeln sich die Fotos
bei jeder Kleinigkeit."

Zwei Anforderungen, die einander zu widersprechen scheinen: **nichts verlieren**
und **nichts verdoppeln**.

## 2. Das Konzept

Der Gegenstand selbst trägt keine Daten — er ist nur eine Identität. Alles
Inhaltliche hängt an einem **Zustand**: eine Momentaufnahme mit Zeitpunkt und
Urheber. Eine Änderung ändert keinen Zustand, sondern **erzeugt einen neuen**;
der alte bleibt unverändert stehen. Anhänge — Bilder, Dateien — werden dabei
**per Verweis** in den neuen Zustand übernommen, nicht kopiert: Derselbe Anhang
kann in vielen Zuständen vorkommen und liegt trotzdem nur einmal.

Damit ist alles unveränderlich — **mit genau einer Ausnahme**: Ein Metadatum,
das nachträglich von einer Maschine ergänzt wird und noch nicht endgültig ist,
darf am **jeweils letzten** Zustand weiterleben, bis der nächste Zustand es
einfriert.

## 3. Begriffe

- **Gegenstand** — die Identität, an der die Geschichte hängt. Trägt selbst
  keine inhaltlichen Daten.
- **Zustand** — eine vollständige Momentaufnahme mit Zeitpunkt und Urheber.
  Unveränderlich, sobald ein weiterer Zustand darauf folgt.
- **Anhang** — eine Datei, die zu einem Zustand gehört und in mehreren Zuständen
  vorkommen kann, ohne mehrfach zu existieren.
- **Übernahme per Verweis** — ein neuer Zustand erbt Anhänge des vorigen, indem
  er auf sie zeigt.
- **Lebendes Metadatum** — die eine Ausnahme: eine Angabe am letzten Zustand,
  die noch ergänzt werden darf.

## 4. Festlegungen

> **Vorbehalt:** F1 bis F4 sind an einem flachen Modell entstanden und dort
> erprobt. Ihre Übertragung auf Objektbäume ist **nicht** entschieden (Q1).

### F1 · Jede Änderung erzeugt einen Zustand

**Was gilt.** Es gibt keinen Vorgang, der einen bestehenden Zustand ändert.
Texte, Anhänge, Kennzeichnungen, auch eine Verlustmeldung — alles erzeugt einen
**neuen** Zustand.

**Warum.** Nur so ist jede Aussage datierbar und einem Urheber zuzuordnen. Ein
änderbarer Datensatz beantwortet die Frage „was gilt jetzt?", aber nicht „was
galt damals?" — und die zweite ist die, wegen der man eine Geschichte führt.

**Naheliegend und verworfen.** *Nur „wichtige" Änderungen versionieren* — die
Grenze ist nicht bestimmbar, und jeder zieht sie um seine eigene Änderung herum.
*Ein Änderungsprotokoll neben dem Datensatz* — zwei Wahrheiten, die
auseinanderlaufen; das Protokoll wird beim ersten Sonderfall unvollständig.

**Woran man merkt, dass es verletzt ist.** Ein Zustand trägt ein Datum, und sein
Inhalt hat sich danach geändert.

### F2 · Anhänge werden übernommen, nicht kopiert

**Was gilt.** Ein neuer Zustand zeigt auf dieselben Anhänge wie sein Vorgänger,
soweit sie weitergelten. Es entsteht keine zweite Datei.

**Warum.** Sonst verdoppelt eine geänderte Textzeile den gesamten
Anhangsbestand. Das ist der eigentliche Grund, warum unveränderliche Historien
oft aufgegeben werden — nicht die Zahl der Datensätze, sondern die der Dateien.

**Naheliegend und verworfen.** *Anhänge kopieren, „dann ist jeder Zustand
autark"* — Autarkie, die niemand braucht, zum Preis unbegrenzten Wachstums.
*Anhänge am Gegenstand statt am Zustand führen* — dann ist nicht mehr erkennbar,
welcher Anhang wann galt.

**Woran man merkt, dass es verletzt ist.** Die Zahl der Dateien wächst mit der
Zahl der Textänderungen.

### F3 · Genau ein lebendes Metadatum — und nur am letzten Zustand

**Was gilt.** Eine maschinell ergänzte Angabe darf am **jeweils letzten**
Zustand nachträglich entstehen und sich ändern. Sobald ein weiterer Zustand
folgt, ist sie eingefroren. Es gibt **eine** solche Angabe, nicht mehrere.

**Warum.** Maschinelle Auswertung braucht Zeit und läuft der Eingabe hinterher.
Ohne diese Ausnahme müsste jedes Ergebnis einen neuen Zustand erzeugen — und die
Geschichte bestünde aus Einträgen, die kein Mensch veranlasst hat. Die Grenze
„nur am letzten" ist das, was die Ausnahme klein hält: Was einmal überholt ist,
wird nie wieder angefasst.

**Naheliegend und verworfen.** *Mehrere lebende Angaben zulassen* — jede
einzelne ist begründbar, und zusammen heben sie F1 auf. *Auswertungsergebnisse
als eigene Zustände* — die Geschichte füllt sich mit Vorgängen, die niemand
veranlasst hat.

**Woran man merkt, dass es verletzt ist.** Ein nicht mehr letzter Zustand hat
sich geändert.

### F4 · Urheber und Herkunft sind Momentaufnahmen, keine Verweise

**Was gilt.** Wer einen Zustand erzeugt hat und von wo, steht als **Text** im
Zustand — nicht als Verweis auf einen Benutzerdatensatz.

**Warum.** Nur so bleibt die Geschichte lesbar, wenn ein Benutzer gelöscht oder
ein Gerät entfernt wird. Ein Verweis würde entweder das Löschen verhindern oder
die Geschichte durchlöchern; beides ist schlechter als ein Name, der stehen
bleibt.

**Naheliegend und verworfen.** *Fremdschlüssel auf den Benutzer* — verhindert
Löschen oder erzeugt tote Verweise. *Gar keine Herkunft festhalten* — dann ist
die Geschichte vollständig und nutzlos.

**Woran man merkt, dass es verletzt ist.** Ein gelöschter Benutzer hinterlässt
Lücken in der Geschichte.

**Tragwerk.** F1 trägt alles. **F2 ist die Bedingung dafür, dass F1 bezahlbar
bleibt** — ohne Übernahme per Verweis wird die Unveränderlichkeit beim ersten
großen Anhang aufgegeben. F3 ist die einzige Ausnahme von F1 und nur deshalb
vertretbar, weil sie eng ist. F4 steht unabhängig.

## 5. Sicherheitsbetrachtung

*Fragment — noch nicht ausgearbeitet.* Zu behandeln sind mindestens: Was ein
unveränderlicher Bestand über Personen dauerhaft festhält (F4 schreibt Namen
fest, und sie lassen sich nicht mehr entfernen); ob eine Löschpflicht mit der
Unveränderlichkeit vereinbar ist; und wie ein lebendes Metadatum (F3) davor
geschützt wird, dass jemand darüber Inhalte austauscht, die bereits angezeigt
werden.

## 6. Was das kostet

*Fragment.* Bekannt sind: mehr Datensätze; die Notwendigkeit, Anhänge getrennt
zu verwalten; die Unmöglichkeit, einen Fehler still zu korrigieren — eine
falsche Eingabe bleibt sichtbar und wird durch einen neuen Zustand richtiggestellt,
nicht überschrieben.

## 7. Geltung und Grenzen

**Randbedingungen.** Ein Gegenstand mit einer überschaubaren Zahl von Änderungen
und Anhängen, die nicht bei jeder Änderung wechseln.

**Kipp-Bedingung, die dieses Fragment offen lässt:** Sobald der Gegenstand
selbst eine **Struktur** hat — ein Baum aus Teilen, die einzeln geändert werden
—, ist nicht entschieden, worauf sich „ein Zustand" bezieht (Q1).

**Nicht gelöst.** Alles, was mit Relationen zu tun hat. Siehe Q1.

## 8. Bewährung

*Entfällt — das Verfahren läuft in einem Projekt des Autors, aber die
Vollzugsmeldung steht aus, und für ein Fragment wäre sie verfrüht.*

## 9. Offene Fragen

**Q1 · Wie überträgt sich das auf Objektbäume?** *Die Frage, wegen der dieses
Dossier ein Fragment ist.* Bei einem flachen Gegenstand ist „ein Zustand"
eindeutig. Hat die Sache eine Baumstruktur — ein Dokument mit Abschnitten, eine
Stückliste mit Baugruppen —, stellt sich die Frage, worauf sich eine Änderung
bezieht:

- **Zustand des Teilbaums:** Die Änderung eines Blattes erzeugt einen neuen
  Zustand nur dort. Die Historie wächst mit der **Änderungsmenge** — aber es
  gibt keinen konsistenten Gesamtschnitt mehr: „Wie sah der ganze Baum am 3.
  März aus?" muss aus Teilständen zusammengesetzt werden.
- **Zustand des ganzen Baums:** Jede Änderung erzeugt einen neuen Gesamtzustand.
  Der konsistente Schnitt ist trivial — aber die Historie wächst mit der
  **Baumgröße** statt mit der Änderungsmenge, und F2 müsste für Teilbäume gelten,
  nicht nur für Anhänge.

Beide Wege haben einen Preis, und die Wahl hängt vermutlich davon ab, ob der
Gesamtschnitt fachlich gebraucht wird. **Der Autor hat Überlegungen dazu, die
noch nicht eingeflossen sind.** Bis sie vorliegen, bleibt dieses Dossier ein
Fragment.

**Q2 · Gilt F3 auch im Baum?** Wenn ein lebendes Metadatum am „letzten" Zustand
hängt — was ist der letzte Zustand in einem Baum, in dem an mehreren Stellen
gleichzeitig gearbeitet wird?

**Q3 · Wie verträgt sich Unveränderlichkeit mit einer Löschpflicht?** F4 schreibt
Namen als Text fest, damit die Geschichte Löschungen übersteht. Genau das macht
sie unlöschbar. Für ein privates System hinnehmbar; sobald personenbezogene
Daten in größerem Umfang anfallen, ist es eine offene Frage.

## 10. Quellen & Verwandtes

*Fragment — Fundstellen sind vor einer Veröffentlichung zu prüfen und mit
Abrufdatum zu versehen.*

- **Ereignisorientierte Datenhaltung** (Event Sourcing) als das verwandte,
  deutlich schwerere Verfahren — es speichert Änderungen statt Zustände und
  errechnet den Stand daraus. Die Abgrenzung gehört ausgearbeitet: Dieses
  Konzept speichert **Zustände**, nicht Ereignisse, und braucht deshalb keine
  Wiedergabe.
- **Vertiefung, nicht Voraussetzung:** Dass ein angenommenes Dokument nie mehr
  geändert, sondern ersetzt wird, ist dieselbe Denkfigur auf einem anderen
  Gegenstand — beschrieben in `immutable-decision-records`
  (<https://github.com/schlingensiepen/proposals/tree/main/immutable-decision-records>).
  Die Verwandtschaft ist auffällig und gehört bei der Fertigstellung geprüft:
  Dort ist die Ersetzung **vollständig**, hier wird per Verweis übernommen — der
  Unterschied hat einen Grund (F2) und sollte benannt werden.
