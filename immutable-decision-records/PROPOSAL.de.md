---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-13
author: Jörn Schlingensiepen
slug: immutable-decision-records
language: de (führend)
question: "Wie hält man Entscheidungen so fest, dass sie in zwei Jahren noch tragen — und dass man sieht, was inzwischen nicht mehr gilt?"
thesis: "Ein angenommenes Entscheidungsdokument wird nie mehr geändert; jede Änderung ist ein neues, vollständiges Dokument, das das alte ersetzt."
---

# Entscheidungen werden ersetzt, nicht geändert

**In einem Satz:** Wer eine getroffene Entscheidung ändern will, schreibt ein
neues, in sich geschlossenes Dokument, das das alte vollständig ersetzt — auch
wenn sich nur ein Satz ändert.

**Kurzthese:** Entscheidungsdokumente werden im Alltag fortgeschrieben, und
genau dadurch verlieren sie ihren Wert: Man sieht am Ende nur den letzten Stand
und weiß nicht mehr, was wann warum galt. Unveränderlichkeit plus vollständige
Ersetzung stellt beides wieder her — den heutigen Stand *und* den Weg dorthin —
zum Preis von mehr Dokumenten und etwas Redundanz.

## 1. Problem

„Ich habe vor einem Jahr entschieden, das so zu bauen. Dass ich es entschieden
habe, weiß ich noch. Was ich damals gegen die naheliegende Alternative hatte,
weiß ich nicht mehr — also denke ich es neu durch, komme mit demselben Kopf
meist auf dasselbe Ergebnis, nur drei Tage später. Und wenn ich ins Dokument
schaue, steht dort ein Satz, den jemand irgendwann korrigiert hat. Ob das die
ursprüngliche Entscheidung war oder eine spätere Reparatur, sehe ich nicht."

Das ist der Normalfall bei fortgeschriebenen Entscheidungsdokumenten. Zwei
Schäden stecken darin, und sie sind verschieden:

- **Der verlorene Grund.** Eine Entscheidung ohne ihre verworfenen Alternativen
  ist eine Meinung. Sie wird beim ersten Widerstand aufgegeben — von einem
  neuen Mitarbeiter, von einem Assistenzsystem, vom eigenen Ich in zwei Jahren.
- **Der unsichtbare Wechsel.** Wird ein Dokument an einer Stelle korrigiert,
  entsteht ein Text, der *aussieht* wie eine Entscheidung, aber in Wahrheit
  zwei Entscheidungen aus verschiedenen Jahren übereinanderlegt. Niemand kann
  ihm ansehen, wo die Naht verläuft.

Der zweite Schaden ist der teurere, weil er nicht auffällt. Ein fehlender Grund
ärgert; eine unsichtbare Naht führt dazu, dass jemand eine längst revidierte
Festlegung umsetzt und dabei glaubt, dem aktuellen Stand zu folgen.

## 2. Das Konzept

Jede Entscheidung bekommt ein eigenes, kurzes Dokument mit fortlaufender Nummer.
Solange es im Entwurf ist, wird daran gearbeitet. In dem Moment, in dem es
angenommen ist, wird sein Inhalt **unveränderlich**: Am Fließtext ändert
niemand mehr etwas. Wer die Entscheidung revidieren will, schreibt ein **neues**
Dokument, das die Frage vollständig neu beantwortet — mit eigener Begründung,
eigenen verworfenen Alternativen, eigenem Titel — und trägt im Kopf beider
Dokumente ein, welches welches ersetzt. Das alte bleibt stehen, wird als
abgelöst markiert und nie gelöscht. Ein Verzeichnis über den ganzen Bestand
zeigt auf einen Blick, was gilt und was nur noch Geschichte ist. Nummern werden
nie wiederverwendet, auch nicht nach einer Löschung im Entwurfsstadium. Damit
gibt es zu jedem Zeitpunkt genau zwei Fragen, die man beantworten kann: *Was
gilt heute?* und *Was galt damals, und warum wurde es aufgegeben?*

## 3. Begriffe

- **Entscheidungsdokument** — ein Text, der **eine** getroffene Entscheidung
  festhält: die Frage, die Optionen, die Wahl, die Begründung, die Folgen.
- **Angenommen** — der Zustand, ab dem die Entscheidung gilt und der Text
  unveränderlich wird. Das Gegenstück ist **Entwurf**: dort wird noch
  geschrieben, verworfen und umgestellt.
- **Ersetzung** — die vollständige Ablösung eines Dokuments durch ein neues.
  Nicht: eine Korrektur, ein Nachtrag, eine Fußnote.
- **Abgelöst** — der Zustand eines Dokuments, dessen Entscheidung nicht mehr
  gilt. Es bleibt lesbar und auffindbar.
- **Kette** — die Folge von Dokumenten zu derselben Sachfrage, verbunden durch
  Ersetzungsvermerke.
- **Verzeichnis** — die eine Übersicht über den gesamten Bestand, aus der der
  Geltungsstand jedes Dokuments hervorgeht.
- **Kopf** — der maschinenlesbare Block am Dateianfang (Status, Datum,
  Beteiligte, Ersetzungsbeziehungen). Er ist Buchhaltung, nicht Aussage.

## 4. Festlegungen

### F1 · Ein Dokument, eine Entscheidung

**Was gilt.** Jede Entscheidung steht in einer eigenen Datei mit eigener,
fortlaufender Nummer; eine Sammeldatei mit allen Entscheidungen gibt es nicht.

**Warum.** Nur eine eigene Datei kann als Ganzes gelten oder als Ganzes
abgelöst werden. In einer Sammeldatei ist der Geltungsstand ein Attribut von
Absätzen — und Absätze haben keinen Status, den ein Werkzeug oder ein Leser
zuverlässig erfasst. Außerdem lässt sich nur so beantworten, wann eine
Entscheidung getroffen wurde: Die Datei trägt ihr eigenes Datum.

**Naheliegend und verworfen.** *Eine Sammeldatei mit Abschnitten je
Entscheidung* — bequem zu durchsuchen und am Anfang übersichtlich, aber sie
erzwingt Fortschreibung: Eine Revision wird zur Korrektur im Absatz, und genau
das soll dieses Konzept verhindern. *Ein Dokument je Themenbereich* („alles zur
Datenhaltung") — die Grenzen verschieben sich mit der Zeit, und jede Revision
trifft dann Nachbarentscheidungen mit, die niemand anfassen wollte.

**Woran man merkt, dass es verletzt ist.** Jemand fragt „gilt der zweite Absatz
noch?" — und niemand kann es aus dem Dokument beantworten.

### F2 · Nach der Annahme ist der Inhalt unveränderlich — Kopf und Dokumentenanfang ausgenommen

**Was gilt.** Unveränderlich ist der **Inhalt**: die Aussage der Entscheidung,
ihre Begründung, ihre Optionen, ihre Folgen. **Ausdrücklich ausgenommen sind
zwei Stellen** — der **Kopf** (Status und die beiden Beziehungslisten) und der
**Anfang des Dokuments**, an dem der Ablösungsvermerk mit dem Verweis auf den
Nachfolger steht. Dazu kommen Tippfehler und tote Verweise. Alles andere ist ein
neues Dokument.

**Warum.** Die Unveränderlichkeit ist das, was die Aussage datierbar macht.
Steht in einem Dokument von 2024 ein Satz von 2026, ist keine einzige Aussage
darin mehr zeitlich verortbar — und damit auch keine Begründung, denn eine
Begründung gilt immer gegenüber einem Kenntnisstand.

**Die beiden Ausnahmen sind keine Bequemlichkeit, sondern die Bedingung dafür,
dass die Beziehung in beide Richtungen geführt werden kann.** Der Rückverweis
vom abgelösten auf das ablösende Dokument entsteht zwangsläufig **später** als
das abgelöste Dokument selbst — oft Jahre später. Ohne diese Ausnahme ließe sich
die Ersetzung nur vorwärts eintragen, und ein abgelöstes Dokument wäre von sich
aus nicht als abgelöst erkennbar (F5). Genau dieses Dokument findet ein Leser
aber zuerst.

Dass es **zwei** Stellen sind und nicht eine, hat einen Grund: Der Kopf wird von
Werkzeugen gelesen, der Vermerk am Anfang von Menschen. Wer ein Dokument
aufschlägt, liest die ersten Zeilen des Textes — nicht den Metadatenblock, und
schon gar nicht bis zu einem Feld ganz unten. Eine Ablösung, die nur im Kopf
steht, ist maschinell korrekt und praktisch unsichtbar.

Beide Eingriffe berühren den Kenntnisstand nicht: Sie halten fest, *was mit* dem
Dokument geschehen ist, nicht *was es sagt*. Der Vermerk am Anfang sagt nichts
über die Entscheidung aus — er sagt, dass sie nicht mehr gilt.

**Naheliegend und verworfen.** *Kleine Präzisierungen erlauben, größere nicht* —
die Grenze ist nicht bestimmbar, und jeder zieht sie so, dass seine eigene
Änderung noch hineinpasst. Nach einem halben Jahr ist das Prinzip aufgeweicht,
ohne dass es jemand beschlossen hätte. *Auch Kopf und Vermerk einfrieren, damit
die Regel ohne Ausnahme auskommt* — klingt sauberer und macht die Ablösung
einseitig: Sie stünde nur im neuen Dokument, das alte wüsste nichts von seinem
Ende. *Änderungen erlauben, aber die Historie im Versionsverwaltungssystem
lassen* — dort steht sie zwar, aber niemand liest sie: Wer ein Dokument
aufschlägt, sieht den letzten Stand, und die Frage „war das schon immer so?"
stellt sich gar nicht erst.

**Woran man merkt, dass es verletzt ist.** Ein Dokument trägt ein Annahmedatum,
und seine letzte Änderung liegt danach und betrifft etwas anderes als Kopf,
Ablösungsvermerk, Tippfehler oder toten Verweis.

### F3 · Ersetzung ist vollständig — „teilweise überholt" gibt es nicht

**Was gilt.** Ein ersetzendes Dokument beantwortet die Frage vollständig neu,
einschließlich der Teile, die unverändert bleiben. Ein Vermerk der Art „gilt
weiter, außer Absatz 3" ist unzulässig.

**Warum.** Wer „teilweise überholt" liest, muss zwei Dokumente gleichzeitig im
Kopf halten und selbst entscheiden, welcher Satz wovon getroffen ist. Das ist
genau die Arbeit, die dieses Konzept dem Leser abnehmen soll — und es ist Arbeit
mit Fehlerrisiko, die jeder Leser erneut leistet, jedes Mal mit womöglich
anderem Ergebnis. Der Schreiber leistet sie einmal.

**Naheliegend und verworfen.** *Ein Delta-Dokument, das nur die Änderung
beschreibt* — spart beim Schreiben und kostet bei jedem Lesen; nach der zweiten
Revision braucht man drei Dokumente, um eine Frage zu beantworten. *Ein Verweis
„im Übrigen gilt das Vorgängerdokument"* — dieselbe Falle in höflicher Form: Er
macht das neue Dokument unvollständig, und beim Lesen der Kette weiß niemand
mehr, welche Fassung des „Übrigen" gemeint war.

**Woran man merkt, dass es verletzt ist.** Eine Frage lässt sich nur
beantworten, indem man zwei oder mehr Dokumente nebeneinanderlegt.

### F4 · Das abgelöste Dokument bleibt stehen — und behält seinen eigenen Titel

**Was gilt.** Ein abgelöstes Dokument wird nicht gelöscht, nicht geleert und
nicht umbenannt. Sein Titel beschreibt weiterhin, was **es** entschieden hat,
nicht was der Nachfolger entscheidet; es bekommt einen Statusvermerk und einen
Verweis auf den Nachfolger.

**Warum.** Das abgelöste Dokument ist der einzige Ort, an dem die aufgegebene
Lösung samt ihrer damaligen Begründung steht. Wer sie löscht, produziert genau
den Zustand, den das Konzept vermeiden soll: eine Entscheidung ohne
nachvollziehbare Vorgeschichte. Und der eigene Titel ist wichtiger, als er
aussieht — trüge das alte Dokument den Titel des neuen, stünden zwei Dokumente
mit demselben Namen im Verzeichnis, und beim Zitieren einer Entscheidung wüsste
niemand, welche gemeint ist.

**Naheliegend und verworfen.** *Abgelöste Dokumente in ein Archivverzeichnis
verschieben* — bricht alle bestehenden Verweise auf sie, und die Kette wird
unlesbar, sobald sie über die Verzeichnisgrenze läuft. *Den Titel auf den neuen
Stand ziehen, damit das Verzeichnis stimmig aussieht* — erzeugt Dubletten im
Register und macht Zitate mehrdeutig; das Register zeigt den Geltungsstand
ohnehin anders (F8).

**Woran man merkt, dass es verletzt ist.** Zwei Einträge im Verzeichnis heißen
gleich — oder ein Verweis auf eine ältere Entscheidung führt ins Leere.

### F5 · Die Ersetzungsbeziehung ist n:m und wird an beiden Enden geführt

**Was gilt.** Jedes Dokument trägt im Kopf zwei **Listen**: was es ersetzt und
wovon es ersetzt wurde. Beide werden beim Anlegen des Nachfolgers in einem Zug
gepflegt.

**Warum.** Die Beziehung ist nicht eins zu eins. Eine Revision kann zwei alte
Entscheidungen zu einer zusammenführen, und eine wachsende Entscheidung kann in
zwei getrennte zerfallen; beides kommt vor, sobald ein Bestand ein paar Jahre
alt ist. Ein einzelnes Feld erzwänge in diesen Fällen eine Notlüge. Und beide
Richtungen sind nötig, weil beide Leserichtungen vorkommen: vorwärts („was ist
aus dieser Entscheidung geworden?") und rückwärts („was hat dieses Dokument
ersetzt, und warum reichte das nicht?").

**Naheliegend und verworfen.** *Nur der Vorwärtsverweis im neuen Dokument* —
dann ist ein abgelöstes Dokument von sich aus nicht als abgelöst erkennbar, und
genau dieses Dokument findet ein Leser zuerst, wenn er im Bestand sucht.
*Einzelwert statt Liste* — verbietet Zusammenführung und Aufteilung; wer sie
später doch braucht, baut sich einen Hilfsvermerk im Fließtext und hat damit die
Beziehung an einen Ort gelegt, an dem kein Werkzeug sie findet.

**Woran man merkt, dass es verletzt ist.** Ein Dokument nennt seinen Nachfolger,
und der Nachfolger nennt es nicht — oder umgekehrt.

### F6 · Nummern werden nie wiederverwendet

**Was gilt.** Eine einmal vergebene Nummer bleibt bei ihrem Dokument, auch wenn
dieses im Entwurfsstadium verworfen wurde. Lücken in der Nummernfolge sind
normal und werden nicht geschlossen.

**Warum.** Nummern werden zitiert — in Besprechungen, in Quelltextkommentaren,
in anderen Dokumenten, in E-Mails. Eine wiederverwendete Nummer macht jedes
frühere Zitat still falsch: Der Verweis zeigt weiterhin auf ein existierendes
Dokument, nur eben auf das falsche. Ein toter Verweis ist harmlos, ein
umgeleiteter ist gefährlich.

**Naheliegend und verworfen.** *Lücken schließen, damit die Folge sauber
aussieht* — kosmetisch, und es verschiebt im Zweifel gleich mehrere Nummern.
*Nummern ganz weglassen und nach Titel zitieren* — Titel werden lang, ändern
sich in der Kette (F4) und lassen sich nicht mündlich abkürzen.

**Woran man merkt, dass es verletzt ist.** Ein älterer Verweis führt auf ein
Dokument, das offensichtlich von etwas anderem handelt.

### F7 · Die verworfenen Alternativen sind Pflichtteil, mit Begründung

**Was gilt.** Jedes Dokument benennt die Optionen, die ernsthaft im Raum
standen, und sagt zu jeder, warum sie es nicht geworden ist.

**Warum.** Ohne diesen Teil schlägt der Nächste die verworfene Lösung wieder
vor — und niemand erinnert den Grund, also wird sie erneut durchgerechnet oder,
schlimmer, übernommen. Der Wert eines Entscheidungsbestands liegt nicht in den
Entscheidungen, die getroffen wurden; die stehen ohnehin im Ergebnis. Er liegt
in denen, die **nicht** getroffen wurden. Das gilt umso mehr, wenn
Assistenzsysteme mitlesen: Sie schlagen zuverlässig die naheliegendste Lösung
vor, und die naheliegendste ist oft genau die, die man einmal begründet
verworfen hat.

**Naheliegend und verworfen.** *Nur die gewählte Option festhalten, die
Alternativen im Gedächtnis lassen* — das Gedächtnis ist der unzuverlässigste
Teil des Systems und der einzige, der nicht gesichert wird. *Alternativen
auflisten, aber ohne Begründung* — eine Liste ohne Gründe lädt geradezu dazu
ein, sie erneut zu prüfen.

**Woran man merkt, dass es verletzt ist.** In einer Besprechung wird ein
Vorschlag gemacht, den der Bestand schon einmal verworfen hat, und niemand
bemerkt es.

### F8 · Ein Verzeichnis zeigt den Geltungsstand des ganzen Bestands

**Was gilt.** Über allen Dokumenten steht eine Übersicht, aus der für jedes
Dokument hervorgeht, ob es gilt oder abgelöst ist und wovon. Sie wird mit jedem
neuen Dokument im selben Zug fortgeschrieben.

**Warum.** Der Bestand wird per Volltextsuche gelesen, und eine Volltextsuche
kennt keinen Status: Sie liefert das abgelöste Dokument genauso wie das
geltende, oft sogar zuerst, weil das ältere länger existiert und häufiger
verlinkt ist. Ohne Verzeichnis ist die Frage „was gilt eigentlich?" nur zu
beantworten, indem man **alle** Dokumente öffnet. Das tut niemand.

**Naheliegend und verworfen.** *Auf den Statusvermerk im Dokument vertrauen* —
notwendig, aber nicht hinreichend: Er beantwortet die Frage erst, wenn man das
richtige Dokument schon gefunden hat. *Das Verzeichnis aus den Köpfen
generieren* — richtig und empfehlenswert, sobald es ein Werkzeug gibt; ohne
Werkzeug ist ein von Hand gepflegtes Verzeichnis besser als keines, und der
Aufwand ist eine Zeile je Entscheidung.

**Woran man merkt, dass es verletzt ist.** Jemand setzt eine Festlegung um, die
seit einem halben Jahr abgelöst ist, und beruft sich dabei auf den Bestand.

**Tragwerk.** F2 trägt alles: Fällt die Unveränderlichkeit, verlieren F3, F4,
F5 und F6 ihren Zweck — sie regeln nur, wie eine Ersetzung aussieht, und ohne
Unveränderlichkeit gäbe es keine Ersetzungen, sondern Korrekturen. F1 ist die
Voraussetzung von F2: Unveränderlich kann nur sein, was als Ganzes gilt. F7 und
F8 stehen unabhängig — sie sind auch für einen fortgeschriebenen Bestand
sinnvoll und lassen sich getrennt übernehmen. Wer nur eine Regel mitnehmen will,
nimmt F7.

## 5. Sicherheitsbetrachtung

Ein Dokumentationsverfahren scheint sicherheitsneutral zu sein. Es ist es nicht:
Unveränderlichkeit gilt auch für das, was versehentlich hineingeraten ist, und
ein falsch gelesener Geltungsstand kann eine aufgegebene Schutzmaßnahme wieder
in Kraft setzen.

| Gefahr | Typischer Hergang | Gegenmaßnahme |
|---|---|---|
| **Geheimnis wird unveränderlich** | Ein Beispiel im Dokument enthält einen echten Schlüssel, Hostnamen oder Zugangsdatensatz. Später fällt es auf — aber der Text darf nicht mehr geändert werden | Vor der Annahme prüfen; Beispiele grundsätzlich mit erfundenen Werten. Tritt der Fall doch ein, **hat die Geheimnisrotation Vorrang vor der Unveränderlichkeit**: Der Schlüssel wird gewechselt, und die Entfernung aus dem Text wird als ausdrückliche, protokollierte Ausnahme behandelt — nicht als stille Korrektur |
| **Abgelöste Sicherheitsentscheidung wird umgesetzt** | Jemand sucht im Bestand nach „Zugriff" und findet zuerst das ältere, ausführlichere Dokument. Es beschreibt einen offenen Lesezugriff, der längst aufgegeben wurde | Statusvermerk **im Dokument selbst**, nicht nur im Verzeichnis (F5), und sichtbar oben, nicht im Kopfblock allein. Ein Leser, der nur den Anfang liest, muss die Ablösung sehen |
| **Assistenzsystem zieht abgelöste Festlegungen heran** | Ein Werkzeug liest den ganzen Ordner in seinen Kontext und gewichtet alle Dokumente gleich | Verzeichnis mit eindeutiger Kennzeichnung (F8) und ein Einstiegshinweis, der ausdrücklich sagt, dass abgelöste Dokumente stehen bleiben — sonst wirkt ihr Vorhandensein wie Geltung |
| **Rückwirkende Umdeutung** | Ein Beteiligter ändert nachträglich die Begründung einer unbequem gewordenen Entscheidung | Die Unveränderlichkeit ist hier selbst die Gegenmaßnahme, sofern der Bestand versioniert liegt: Jede Änderung ist als solche sichtbar |

**Wogegen das Konzept nicht hilft:** Es schützt nicht davor, dass eine
Entscheidung sachlich falsch ist, und es ersetzt keine Zugriffskontrolle. Ein
Entscheidungsbestand verrät regelmäßig mehr über die Architektur eines Systems
als dessen Quelltext — wer ihn veröffentlicht, sollte das absichtlich tun.

## 6. Was das kostet

- **Mehr Dokumente.** Ein Bestand, der fortgeschrieben zwanzig Abschnitte hätte,
  hat hier vierzig Dateien, von denen ein Drittel nicht mehr gilt.
- **Ein Satz Änderung kostet ein ganzes Dokument.** Das ist der wunde Punkt: Der
  Aufwand ist spürbar, und die Versuchung, „nur eben schnell" im alten Dokument
  zu korrigieren, ist an genau dieser Stelle am größten — bei kleinen
  Änderungen, wo das Verfahren am teuersten wirkt.
- **Redundanz zwischen Vorgänger und Nachfolger.** Große Teile werden
  abgeschrieben. Wer beide liest, liest vieles zweimal.
- **Das Verzeichnis ist eine Pflicht, die man vergessen kann.** Wird es
  vergessen, kippt der Nutzen des ganzen Verfahrens, ohne dass etwas kaputt
  aussieht.
- **Die Kette will gelesen werden.** Wer die Vorgeschichte einer Entscheidung
  verstehen will, öffnet drei Dokumente statt eines Absatzes.

## 7. Geltung und Grenzen

**Randbedingungen.** Der Bestand liegt als Textdateien in einem versionierten
Verzeichnis, eine Datei je Entscheidung, mit einem maschinenlesbaren Kopf. Mehr
braucht es nicht — kein Werkzeug, keine Datenbank, keine Weboberfläche. Wer
schreibt, muss bereit sein, ein Dokument zu Ende zu schreiben, statt es
anzureißen.

**Kipp-Bedingungen.** Die Antwort wird falsch, wenn (a) Entscheidungen sich
**wöchentlich** ändern — dann ersetzt man mehr, als man entscheidet, und der
Bestand wird zur Kette ohne Substanz; (b) die Entscheidungen gar keine sind,
sondern Vorgaben von außen, die man nur notiert — dann fehlt der Kern (F7), und
eine schlichte Liste tut es auch; (c) ein Bestand so groß wird, dass ein von
Hand gepflegtes Verzeichnis nicht mehr stimmt — ab dieser Grenze braucht F8
Werkzeugunterstützung, sonst wird die Übersicht selbst zur Fehlerquelle.

**Nicht gelöst.** Dieses Konzept sagt nichts darüber, **was** zu bauen ist — es
ersetzt keine Spezifikation und keine Anforderungsdokumentation. Es beschreibt
auch nicht, wie ein Bestand **entsteht**, wenn Entscheidungen jahrelang nur
mündlich getroffen wurden; die nachträgliche Rekonstruktion ist eine eigene
Aufgabe mit eigenen Fallen. Und es regelt nicht, wer entscheiden darf.

## 8. Bewährung

**Dieser Mechanismus ist nicht neu, und das ist sein bester Beleg.** Er ist der
Kern dessen, was seit 2011 als *Architecture Decision Record* praktiziert wird:
Schon die ursprüngliche Beschreibung verlangt, ein angenommenes Dokument nicht
mehr zu ändern, sondern es durch ein neues zu ersetzen. Wer je mit ADRs
gearbeitet hat, kennt das Verfahren — nur meist als Eigenheit eines Formats,
nicht als eigenständige Antwort auf eine Frage.

Genau das ist der Grund, warum es hier als eigenes Dossier steht. Die Frage
„wie halte ich Entscheidungen nachvollziehbar?" ist **nicht auf
Architekturentscheidungen beschränkt** — sie stellt sich bei Produkt-,
Beschaffungs-, Betriebs- und redaktionellen Entscheidungen genauso, und dort
hilft der Mechanismus, während das Vokabular („Architecture Decision Record")
in die Irre führt und das dazugehörige Dateiformat überdimensioniert ist. Was
dieses Dossier beiträgt, ist nicht die Erfindung, sondern die **Ablösung des
Mechanismus vom Format**: Er lässt sich auf jeden Entscheidungslog anwenden,
und die Festlegungen F1 bis F8 sagen, was dabei tragend ist und was nicht.

Die eigene Erfahrung damit stammt aus einem privaten Projekt des Autors mit
einem Bestand von siebenunddreißig Dokumenten, davon neun abgelöst. Es entstand nicht auf der
grünen Wiese, sondern durch **Umzug aus einer Sammeldatei** — und dieser Umzug
ist zugleich die belastbarste Erfahrung damit, weil er die Schwächen des
Vorzustands sichtbar machte: In der Sammeldatei waren Revisionen als
Zusatzabsätze unter der ursprünglichen Entscheidung gelandet („Revision vom
…"), sodass mehrere Einträge gleichzeitig gültig und ungültig waren, je nach
Absatz.

Zwei Erfahrungen aus dem Umzug haben Festlegungen dieses Konzepts geschärft:

- Der ausführende Bearbeiter fand im Umzugsplan eine **falsche Gesamtzahl** —
  der Plan verlangte im Text ein Dokument, das seine eigene Summe nicht
  mitzählte. Wer der Zahl gefolgt wäre statt dem Text, hätte eine Entscheidung
  samt Begründung stillschweigend verloren. Daraus die Regel für jeden Umzug:
  **im Zweifel behalten.**
- Ein abgelöstes Dokument hatte zunächst den Titel seines Nachfolgers bekommen,
  damit das Verzeichnis „stimmig" aussieht. Das Ergebnis waren zwei gleich
  benannte Einträge und mehrdeutige Zitate — daraus F4.

> **Beispiel** (aus demselben Projekt, durchgehend): Für die Gegenstände einer
> Inventarverwaltung wurde entschieden, dass ihre **sichtbare Nummer ein
> Identitätsmerkmal** ist — sie identifiziert den Gegenstand, ist eindeutig und
> ändert sich nicht. Das Dokument trug den Titel „Sichtbare Nummer:
> Identitätsattribut, nicht Verlaufsdatum", Begründung: Wer eine Nummer liest,
> soll damit genau einen Gegenstand finden.
>
> Ein halbes Jahr später zeigte sich, dass Nummern in der Praxis nachträglich
> vergeben und korrigiert werden — Etiketten fallen ab, Nummernkreise werden
> umgestellt. Die Entscheidung war falsch. Es entstand **kein Zusatzabsatz** im
> alten Dokument, sondern ein neues mit eigenem Titel („Sichtbare Nummer:
> eigenes Attribut, versioniert im Verlauf"), eigener vollständiger Begründung
> und einer eigenen Liste verworfener Alternativen — darunter die alte
> Entscheidung selbst, jetzt mit dem Grund ihres Scheiterns.
>
> Im Kopf des neuen Dokuments steht `ersetzt: [0011]`, im Kopf des alten
> `ersetzt-durch: [0031]` und `status: abgelöst`; im Verzeichnis ist die Zeile
> von 0011 durchgestrichen und trägt den Vermerk „↳ abgelöst von 0031". Wer
> heute nach der Nummernvergabe sucht, findet beide Dokumente — und weiß nach
> zwei Sekunden, welches gilt und dass es einen Vorgänger gab, den man aus
> gutem Grund aufgegeben hat.

Der Status dieses Dossiers bleibt **Entwurf**: Ein Bestand dieser Größe und ein
Betrieb von wenigen Monaten sind zu wenig für „Bewährt". Insbesondere die
Kipp-Bedingung (a) — hohe Änderungsfrequenz — wurde noch nicht erlebt.

## 9. Offene Fragen

**Q1** Ab welcher Bestandsgröße trägt ein von Hand gepflegtes Verzeichnis nicht
mehr? Die Vermutung des Autors liegt bei etwa fünfzig Dokumenten; belegt ist das
nicht. Gesucht sind Erfahrungswerte aus größeren Beständen.

**Q2** Wie geht man mit einer Entscheidung um, die sehr lang ist und von der
sich nur ein kleiner Teil ändert? F3 verlangt vollständiges Neuschreiben, und
bei einem zehnseitigen Dokument ist das eine echte Hürde — mit dem Risiko, dass
die Entscheidung deshalb *gar nicht* dokumentiert wird. Ist die richtige Antwort
„solche Dokumente sind ohnehin zu groß und hätten geteilt gehört", oder braucht
es hier eine Ausnahme?

**Q3** Darf es ein Dokument geben, das **mehrere** frühe, nie einzeln
dokumentierte Entscheidungen sammelt — als Chronik am Anfang eines Bestands?
Das verletzt F1 offensichtlich. Der Autor hat es zugelassen, weil die
Alternative gewesen wäre, ein Dutzend Entscheidungen rückwirkend zu erfinden,
für die es keine belastbare Erinnerung gab. Ob das eine legitime Ausnahme oder
der erste Riss ist, ist offen.

**Q4** Wie hält man Verweise aus dem Quelltext auf Entscheidungsnummern aktuell,
wenn eine Entscheidung abgelöst wird? Ein Kommentar, der auf die alte Nummer
zeigt, ist nach F6 nicht falsch — aber er zeigt eben auf das abgelöste Dokument.

## 10. Quellen & Verwandtes

- **MADR — Markdown Any Decision Records**, Format-Standard für
  Entscheidungsdokumente, Fassung 4.0.0: <https://adr.github.io/madr/>
  (abgerufen 2026-08-13). Dieses Konzept benutzt MADR als Dateiformat und
  erweitert es um die beiden Beziehungsfelder als Listen (F5).
- **Michael Nygard, „Documenting Architecture Decisions"** (2011), die
  ursprüngliche Beschreibung des Verfahrens einschließlich des Grundsatzes,
  angenommene Dokumente nicht mehr zu ändern, sondern zu ersetzen:
  <https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions>
  (abgerufen 2026-08-13).
- **Übersicht zu Entscheidungsdokumentation**, Sammlung von Vorlagen und
  Praxisberichten: <https://adr.github.io/> (abgerufen 2026-08-13).
- **Vertiefung, nicht Voraussetzung — die konkrete Form:** Das Schwester-Dossier
  `decision-records-with-madr`
  (<https://github.com/schlingensiepen/proposals/tree/main/decision-records-with-madr>)
  beschreibt, wie dieser Mechanismus in MADR aussieht: welche Kopffelder, welche
  Abschnitte, welcher Dateiname, wie die Ersetzung eingetragen wird — mit einer
  kopierfertigen Vorlage. Wer diesen Mechanismus auf einen Bestand von
  Architektur- oder Softwareentscheidungen anwendet, findet dort die fertige
  Form. **Dieses Dossier ist ohne jenes vollständig benutzbar** — der Mechanismus
  ist bewusst formunabhängig beschrieben und gilt für jeden Entscheidungslog.
