---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-13
author: Jörn Schlingensiepen
slug: decision-records-with-madr
language: de (führend)
question: "Wie sieht ein Entscheidungsdokument konkret aus — welche Felder, welche Abschnitte, welcher Dateiname?"
thesis: "MADR wird unverändert übernommen und an genau einer Stelle erweitert: Die Ersetzungsbeziehung steht als zwei Listen im Kopf."
---

# MADR benutzen — ohne es umzubauen

**In einem Satz:** Ein Entscheidungsdokument folgt MADR 4.0.0 unverändert; die
einzige Erweiterung sind zwei Listenfelder im Kopf, die festhalten, was dieses
Dokument ersetzt und wovon es ersetzt wurde.

**Kurzthese:** Wer Entscheidungen dokumentieren will, baut sich meist erst ein
eigenes Format und merkt nach zwanzig Dokumenten, dass es an denselben Stellen
klemmt wie das fertige. MADR ist dieses fertige Format; es kostet nichts, es
kennt jeder, und es lässt sich mit einer einzigen Erweiterung so ergänzen, dass
auch Revisionen sauber abgebildet sind. Der Preis ist ein etwas geschwätziges
Dokument und ein Kopf, den man von Hand pflegt.

## 1. Problem

„Ich habe verstanden, dass ich meine Entscheidungen aufschreiben soll. Aber wie
sieht das Ding aus? Was steht oben drin, was gehört in den Text, wie heißt die
Datei, und woran erkenne ich später, dass diese Entscheidung inzwischen von
einer anderen abgelöst wurde? Ich fange an, ein Format zu erfinden, und weiß
schon beim dritten Dokument nicht mehr, ob ich es beim ersten genauso gemacht
habe."

Dahinter stecken zwei verschiedene Fragen, die gern vermischt werden. Die eine
ist **inhaltlich**: Was muss in einer Entscheidung stehen, damit sie in zwei
Jahren noch trägt? Die andere ist **formal**: Wie sieht die Datei aus, damit ein
Bestand aus hundert Dokumenten noch durchsuchbar, sortierbar und maschinell
auswertbar ist. Dieses Dossier beantwortet die formale Frage — und es beantwortet
sie so, dass die inhaltliche nicht dagegen arbeitet.

Der wunde Punkt ist dabei ausgerechnet die Stelle, die kein verbreitetes Format
befriedigend löst: **die Ersetzung**. Entscheidungen werden revidiert; wenn das
Format dafür nur ein einzelnes Feld vorsieht oder gar keines, landet die
Information im Fließtext, wo kein Werkzeug sie findet und kein Leser sie
zuverlässig sieht.

## 2. Das Konzept

Jede Entscheidung ist eine Markdown-Datei nach **MADR 4.0.0**, unverändert
übernommen: ein YAML-Kopf mit Status, Datum und Beteiligten, darunter Kontext
und Problem, betrachtete Optionen, die Entscheidung mit Begründung und die
Folgen. Der Dateiname beginnt mit einer vierstelligen Nummer und trägt danach
einen sprechenden Kurztitel in Kleinbuchstaben. Ergänzt wird MADR an **genau
einer** Stelle: Der Kopf bekommt zwei zusätzliche Felder, `supersedes` und
`superseded-by`, beide als **Listen**, weil eine Revision mehrere Entscheidungen
zusammenführen und eine Entscheidung in mehrere zerfallen kann. Im Fließtext
kommt ein Pflichtabschnitt hinzu, der die verworfenen Alternativen mit ihrem
Grund ausschreibt — die Optionen erscheinen damit zweimal: einmal als Liste zum
Überfliegen, einmal als Absatz mit Begründung. Über allen Dokumenten steht ein
Verzeichnis als Tabelle. Und im Bestand liegt eine Vorlage, die kopiert wird,
statt dass jemand die Struktur aus dem Gedächtnis nachbaut.

**Diese Dokumente werden einem Mechanismus unterzogen, der älter ist als das
Format und nicht zu ihm gehört:** Ein angenommenes Dokument wird inhaltlich
nicht mehr geändert, eine Revision ist ein neues Dokument, das das alte ersetzt,
und das alte bleibt stehen. Das ist der eigentliche Zweck der Übung — die
Nachvollziehbarkeit einer Entscheidungsfolge —, und es ist zugleich die
Daseinsberechtigung von Entscheidungsdokumenten überhaupt. Dieses Dossier
beschreibt die **Form**, die dieser Mechanismus in MADR annimmt; wo eine
Festlegung hier auf ihm ruht, steht er ausgeschrieben (Begriff „Angenommen",
F3, F9). Seine vollständige Begründung als eigenständiger Mechanismus — auch für
Entscheidungslogs ohne MADR und ohne Architekturbezug — ist ein Gegenstand für
sich; Abschnitt 10 nennt sie. **Voraussetzung ist sie nicht:** Was dieses
Dossier braucht, steht in diesem Dossier.

## 3. Begriffe

- **MADR** — *Markdown Any Decision Records*, ein verbreitetes, offenes Format
  für Entscheidungsdokumente; Markdown-Datei mit YAML-Kopf und festen
  Abschnitten.
- **Kopf** (Frontmatter) — der YAML-Block zwischen zwei Zeilen aus drei
  Bindestrichen am Dateianfang. Maschinenlesbar.
- **Status** — der Zustand eines Dokuments: Entwurf, angenommen, abgelöst.
- **Angenommen** — der Zustand, ab dem die Entscheidung gilt. Mit ihm wird der
  **Inhalt** des Dokuments unveränderlich: Aussage, Begründung, Optionen und
  Folgen werden nicht mehr geändert. Ausgenommen sind genau zwei Stellen — der
  Kopf (F3) und der Anfang des Dokuments, an den der Ablösungsvermerk kommt
  (F9) — sowie Tippfehler und tote Verweise. *Der Grund:* Nur so bleibt
  datierbar, was wann galt. Steht in einem Dokument von 2024 ein Satz von 2026,
  ist keine Aussage darin mehr zeitlich verortbar — und damit auch keine
  Begründung, denn eine Begründung gilt immer gegenüber einem Kenntnisstand.
  Wer revidieren will, schreibt ein neues Dokument, das das alte ersetzt.
- **Ersetzungsbeziehung** — die Verbindung zwischen einem abgelösten Dokument
  und dem, das es ablöst; hier in beiden Richtungen geführt.
- **Bestand** — die Gesamtheit aller Entscheidungsdokumente eines Projekts.
- **Verzeichnis** — die Übersichtsdatei über den Bestand, aus der der
  Geltungsstand jedes Dokuments hervorgeht.
- **Vorlage** — eine mitgelieferte Musterdatei, die für ein neues Dokument
  kopiert wird (Anlage dieses Dossiers, siehe Abschnitt 8).

## 4. Festlegungen

### F1 · MADR 4.0.0 wird unverändert übernommen

**Was gilt.** Das Format ist MADR in der Fassung 4.0.0 — Markdown mit YAML-Kopf,
Abschnitte in der dort vorgesehenen Reihenfolge. Es wird nicht umbenannt, nicht
umsortiert und nicht „an die eigenen Bedürfnisse angepasst".

**Warum.** Ein eigenes Format kostet dreimal: beim Erfinden, beim Erklären und
beim Umziehen, wenn später doch ein Werkzeug ins Spiel kommt, das ein
verbreitetes Format erwartet. MADR ist verbreitet genug, dass Werkzeuge und
Menschen es kennen, und schlank genug, dass es sich nicht in den Weg stellt.
Vor allem aber ist es **fertig**: Die Fragen, an denen ein selbstgebautes Format
nach zwanzig Dokumenten klemmt — wohin mit den Optionen, wohin mit den Folgen,
was gehört in den Kopf —, sind dort bereits entschieden.

**Naheliegend und verworfen.** *Ein eigenes, minimales Format („Titel, Grund,
fertig")* — am Anfang schneller, aber es fehlen genau die Felder, die später
teuer werden: Status, Datum, Optionen. *Nygards ursprüngliches Format ohne
YAML-Kopf* — historisch die Wurzel und immer noch tragfähig, aber ohne
maschinenlesbaren Kopf ist kein Verzeichnis automatisch erzeugbar und kein
Status abfragbar. *Ein ADR-Werkzeug mit eigenem Format und eigener Ablage* —
bindet den Bestand an ein Programm; die Dokumente sollen mit einem Texteditor
lesbar und schreibbar bleiben.

**Woran man merkt, dass es verletzt ist.** Zwei Dokumente desselben Bestands
haben unterschiedliche Abschnittsüberschriften, und niemand weiß, welches die
Vorlage war.

### F2 · Die einzige Erweiterung: beide Ersetzungsrichtungen als Listen

**Was gilt.** Der Kopf trägt zwei zusätzliche Felder, `supersedes` und
`superseded-by`, beide immer als **Liste** — auch wenn sie leer ist oder einen
einzigen Eintrag hat.

**Warum.** MADR kennt einen Status „superseded", aber die Beziehung selbst ist
im Format nicht sauber abgelegt; sie landet üblicherweise als Satz im Text. Dort
findet sie kein Werkzeug, und beim Lesen sieht man sie nur, wenn man an der
richtigen Stelle liest. Als Kopffeld ist sie abfragbar — und **als Liste**, weil
die Beziehung n:m ist: Eine Revision kann zwei alte Entscheidungen
zusammenführen, und eine gewachsene Entscheidung kann in zwei zerfallen. Beides
kommt vor, sobald ein Bestand ein paar Jahre alt ist; ein Einzelwert erzwänge in
diesen Fällen eine Notlüge. Dass es **beide** Richtungen sind, hat einen
praktischen Grund: Wer im Bestand sucht, findet zuerst das ältere Dokument, weil
es länger existiert und häufiger verlinkt ist — es muss von sich aus sagen
können, dass es abgelöst wurde.

**Naheliegend und verworfen.** *Nur `superseded-by`, den Rückweg über eine Suche
lösen* — funktioniert, solange der Bestand klein ist, und scheitert leise, sobald
jemand ein Dokument umbenennt. *Die Beziehung nur im Verzeichnis führen* — dann
ist ein einzeln herausgegriffenes Dokument nicht mehr beurteilbar, und genau so
werden Dokumente gelesen: einzeln, aus einer Suche heraus. *Einzelwerte statt
Listen* — verbietet Zusammenführung und Aufteilung, und wer sie später braucht,
schreibt sich einen Hilfsvermerk in den Text.

**Woran man merkt, dass es verletzt ist.** Ein Kopf trägt `superseded-by` als
einzelne Zeichenkette statt als Liste — oder die Ersetzung steht nur im
Fließtext.

### F3 · Der Kopf ist Buchhaltung, nie Inhalt

**Was gilt.** In den Kopf gehören ausschließlich Verwaltungsangaben: Status,
Entscheidungsdatum, Beteiligte, die beiden Beziehungslisten. Keine Begründung,
keine Zusammenfassung, keine Entscheidung.

**Warum.** Der Inhalt eines Dokuments ist nach der Annahme unveränderlich —
Aussage, Begründung, Optionen, Folgen werden nicht mehr angefasst, weil sonst
nicht mehr datierbar ist, was wann galt. Der Kopf ist eine von genau zwei
Ausnahmen davon: Der Status wechselt, der Rückverweis kommt hinzu (die zweite
ist der Ablösungsvermerk, F9). Stünde im Kopf auch Inhalt, wäre diese Ausnahme
nicht mehr sauber abgrenzbar, und die Unveränderlichkeit wäre praktisch
aufgehoben: Wer eine
Zusammenfassung im Kopf „aktualisiert", hat die Aussage geändert, ohne den Text
anzufassen. Umgekehrt gilt: Was im Kopf steht, wird von Werkzeugen ausgewertet
und darf deshalb keine Nuance enthalten, die nur ein Mensch versteht.

**Naheliegend und verworfen.** *Eine einzeilige Zusammenfassung im Kopf, damit
Übersichten sie anzeigen können* — verlockend, aber sie ist Inhalt und altert
mit ihm; das Verzeichnis kann den Titel zeigen, und der steht in der
Überschrift. *Schlagwörter im Kopf* — harmlos, solange sie nur sortieren; sobald
jemand daraus ableitet, worum es geht, ersetzen sie den Text.

**Woran man merkt, dass es verletzt ist.** Man kann die Entscheidung aus dem
Kopf allein beantworten — oder ein Kopffeld wurde geändert, weil sich die
Sachlage geändert hat, nicht der Status.

### F4 · Der Dateiname trägt Nummer und sprechenden Titel

**Was gilt.** Dateinamen haben die Form `<nnnn>-<kurztitel>.md`: vierstellige
laufende Nummer, Bindestrich, Kurztitel in Kleinbuchstaben mit Bindestrichen,
Umlaute ausgeschrieben. Der Name wird nach der Annahme nicht mehr geändert, und
**eine Nummer wird nie wiederverwendet** — auch keine, die durch einen
verworfenen Entwurf frei geworden ist. Lücken in der Folge bleiben stehen.

**Warum.** Die Nummer sortiert und ist zitierbar, der Titel macht die
Verzeichnisliste ohne Öffnen lesbar — man sieht den ganzen Bestand als
Inhaltsverzeichnis. Vier Stellen, weil dreistellige Nummern bei tausend
Dokumenten die Sortierung brechen und der Umbau dann jeden Verweis trifft.
**Keine Wiederverwendung**, weil Nummern zitiert werden — in Besprechungen, in
Quelltextkommentaren, in anderen Dokumenten. Eine wiederverwendete Nummer macht
jedes frühere Zitat still falsch: Der Verweis zeigt weiterhin auf ein
existierendes Dokument, nur eben auf das falsche. Ein toter Verweis ist harmlos,
ein umgeleiteter ist gefährlich.
Kleinbuchstaben und ausgeschriebene Umlaute, damit Dateisysteme und Kodierungen
nie zum Thema werden — ein Bestand wandert zwischen Rechnern, und ein `ä` im
Dateinamen ist der Unterschied zwischen „funktioniert" und „funktioniert
meistens".

**Naheliegend und verworfen.** *Nur die Nummer (`0031.md`)* — kürzer, aber die
Verzeichnisliste ist dann eine Zahlenkolonne ohne Auskunft. *Nur der Titel ohne
Nummer* — die Sortierung wird alphabetisch und damit sinnlos, und ein Zitat wird
lang. *Datum statt Nummer* — zwei Entscheidungen an einem Tag kollidieren, und
das Umbenennen bei einer Verschiebung ist verboten (siehe oben).

**Woran man merkt, dass es verletzt ist.** Die Verzeichnisliste ist alphabetisch
sortiert statt chronologisch — oder ein Verweis auf einen Dateinamen ist tot,
weil jemand den Titel geschärft hat.

### F5 · Fünf Pflichtabschnitte in fester Reihenfolge

**Was gilt.** Jedes Dokument hat, in dieser Folge: **Kontext und Problem** ·
**Betrachtete Optionen** · **Entscheidung** (mit Begründung und Abwägung) ·
**Folgen** · **Verworfen und warum**. Kein Abschnitt entfällt; ist einer leer,
steht dort, warum.

**Warum.** Feste Reihenfolge macht den Bestand quer lesbar — man liest solche
Dokumente selten von vorn bis hinten, sondern springt zu der einen Auskunft, die
man braucht („was war noch mal die Alternative?"). Das geht nur, wenn dieselbe
Auskunft in jedem Dokument an derselben Stelle steht. Und ein Pflichtabschnitt
greift genau dort, wo man von selbst nicht drandenkt: **Folgen** werden fast
immer vergessen, weil sie im Moment der Entscheidung uninteressant sind und ein
Jahr später das Einzige, was zählt.

**Naheliegend und verworfen.** *Abschnitte weglassen, wenn nichts dazu zu sagen
ist* — ein weggelassener Abschnitt sieht aus wie ein vergessener; ein begründet
leerer ist selbst eine Aussage. *Freie Struktur je nach Entscheidung* — bequem
beim Schreiben, teuer bei jedem Lesen, und nach dreißig Dokumenten hat der
Bestand dreißig Formen.

**Woran man merkt, dass es verletzt ist.** Man muss ein Dokument ganz lesen, um
die verworfenen Alternativen zu finden.

### F6 · Die Optionen erscheinen zweimal — als Liste und als Begründung

**Was gilt.** „Betrachtete Optionen" nennt die Alternativen knapp und markiert
die gewählte. „Verworfen und warum" schreibt zu jeder verworfenen den Grund aus.
Diese Doppelung ist gewollt.

**Warum.** Die beiden Abschnitte bedienen zwei verschiedene Leser. Der eine
überfliegt und will in fünf Sekunden wissen, was zur Wahl stand; der andere
kommt mit einem konkreten Vorschlag und will wissen, warum genau der schon
einmal durchgefallen ist. Presste man beides in einen Abschnitt, verlöre der
erste die Übersicht und der zweite die Begründung — kurze Liste **und**
ausgeschriebener Grund passen nicht in dieselbe Zeile. Der zweite Abschnitt ist
dabei der wertvollere: Der Wert eines Bestands liegt nicht in den Entscheidungen,
die getroffen wurden, sondern in denen, die man begründet **nicht** getroffen
hat.

**Naheliegend und verworfen.** *Nur die Liste, Gründe in Klammern dahinter* —
Klammern erzwingen Kürze, und Kürze ist bei einer Begründung das Falsche. *Nur
den Begründungsabschnitt* — dann fehlt die Übersicht, und man muss vier Absätze
lesen, um drei Optionen zu kennen.

**Woran man merkt, dass es verletzt ist.** Eine Option steht in der Liste, aber
in „Verworfen und warum" fehlt sie — dann ist unklar, ob sie geprüft oder
vergessen wurde.

### F7 · Kopfschlüssel englisch, Fließtext in einer Sprache für den ganzen Bestand

**Was gilt.** Die Schlüssel im Kopf (`status`, `date`, `decision-makers`,
`supersedes`, …) bleiben englisch, wie MADR sie vorgibt. Der Fließtext steht in
einer Sprache, und zwar in derselben für den gesamten Bestand.

**Warum.** Die Kopfschlüssel sind Teil des Formats, nicht des Inhalts —
übersetzt man sie, ist es nicht mehr MADR, und jedes Werkzeug, das den Kopf
liest, geht kaputt. Der Fließtext dagegen richtet sich an Menschen und darf die
Sprache sprechen, in der entschieden wurde: Eine Begründung in einer
Fremdsprache verliert genau die Nuancen, wegen derer man sie aufgeschrieben hat.
Entscheidend ist die **Einheitlichkeit** — ein halb übersetzter Bestand ist
schlechter als jede der beiden reinen Varianten, weil man bei jeder Suche in
zwei Sprachen suchen muss.

**Naheliegend und verworfen.** *Alles auf Englisch, weil Software englisch ist* —
richtig, sobald Mitarbeit von außen erwartet wird; sonst zahlt man
Ausdruckspräzision für einen Nutzen, den niemand einlöst. *Zweisprachige
Dokumente* — doppelter Pflegeaufwand bei einem Dokument, das nach der Annahme
ohnehin nicht mehr angefasst werden soll.

**Woran man merkt, dass es verletzt ist.** Eine Volltextsuche nach einem
Fachbegriff findet die Hälfte des Bestands nicht.

### F8 · Eine Vorlage liegt im Bestand und wird kopiert

**Was gilt.** Im Bestand liegt eine Musterdatei mit allen Kopffeldern und allen
Abschnitten samt eingebetteten Hinweisen, was hineingehört. Ein neues Dokument
entsteht durch Kopieren dieser Datei, nicht durch Nachbauen aus dem Gedächtnis
oder Abschreiben vom letzten Dokument.

**Warum.** Ohne Vorlage nimmt jeder das zuletzt geschriebene Dokument als
Muster — und erbt dessen Eigenheiten mitsamt seinen Fehlern. Nach zehn
Generationen hat der Bestand eine Form, die niemand beschlossen hat. Eine
Vorlage ist außerdem der einzige Ort, an dem die Formatregeln stehen können,
ohne dass jemand sie suchen muss: Sie stehen als Kommentar dort, wo sie gebraucht
werden. Das gilt doppelt, wenn Assistenzsysteme mitschreiben — sie kopieren
zuverlässig, was sie vorfinden.

**Naheliegend und verworfen.** *Die Regeln in eine Anleitung schreiben* — nötig
für das Warum, aber beim Schreiben schlägt niemand eine Anleitung auf. *Das
letzte Dokument als Muster nehmen* — genau der Weg, auf dem sich Abweichungen
vererben.

**Woran man merkt, dass es verletzt ist.** Ein neues Dokument hat ein Kopffeld
weniger als die Vorlage, und niemandem fällt es auf.

### F9 · Der Ablösungsvermerk steht am Anfang des abgelösten Dokuments

**Was gilt.** Wird ein Dokument abgelöst, kommt unter seine Überschrift, **vor**
den ersten Abschnitt, ein sichtbarer Vermerk mit dem Verweis auf den Nachfolger.
Er ist neben dem Kopf die zweite und letzte Stelle, an der ein angenommenes
Dokument noch verändert wird. Am übrigen Text ändert sich nichts.

**Warum.** Der Rückverweis im Kopf (F2) ist maschinenlesbar und für Menschen
praktisch unsichtbar: Wer ein Dokument aufschlägt, liest die ersten Zeilen des
**Textes** — nicht den Metadatenblock, und schon gar nicht ein Feld darin ganz
unten. Eine Ablösung, die nur im Kopf steht, ist formal korrekt und wird
trotzdem übersehen; und übersehen wird sie ausgerechnet bei dem Dokument, das
ein Leser zuerst findet, weil es älter, ausführlicher und häufiger verlinkt ist.

Dass beide Eingriffe — Kopf und Anfang — überhaupt erlaubt sind, hat einen
zwingenden Grund: **Der Rückverweis entsteht später als das Dokument, das ihn
trägt**, oft Jahre später. Ohne diese Ausnahme ließe sich die Ersetzung nur
vorwärts eintragen, im neuen Dokument, und das alte wüsste nichts von seinem
eigenen Ende. Beide Eingriffe berühren die Aussage nicht: Sie halten fest, *was
mit* dem Dokument geschehen ist, nicht *was es sagt*.

**Naheliegend und verworfen.** *Nur den Kopf pflegen* — maschinell korrekt,
praktisch unsichtbar; siehe oben. *Den Vermerk ans Ende setzen* — dort steht er
hinter dem, wovor er warnen soll. *Den Text des abgelösten Dokuments kürzen oder
durch einen Hinweis ersetzen* — zerstört genau den Beleg, worin die Entscheidung
bestand und warum sie damals richtig schien; das abgelöste Dokument ist der
einzige Ort, an dem das steht.

**Woran man merkt, dass es verletzt ist.** Jemand zitiert eine Festlegung aus
einem Dokument, das seit Monaten abgelöst ist — und hat dessen Kopf nie gesehen.

### F10 · Das Verzeichnis ist eine Tabelle mit Geltungsstand

**Was gilt.** Über allen Dokumenten steht eine Übersichtsdatei: je Dokument eine
Zeile mit Nummer, Titel, Entscheidungsdatum und Beziehung. Abgelöste Zeilen sind
als solche gekennzeichnet — durchgestrichen und mit dem Verweis auf den
Nachfolger. Das Verzeichnis wird mit jedem neuen Dokument im selben Zug
fortgeschrieben.

**Warum.** Der Bestand wird per Volltextsuche gelesen, und eine Volltextsuche
kennt keinen Status: Sie liefert das abgelöste Dokument genauso wie das
geltende, oft sogar zuerst. Ohne Verzeichnis ist die Frage „was gilt
eigentlich?" nur zu beantworten, indem man **alle** Dokumente öffnet — das tut
niemand. Damit ist die Ersetzung an drei Stellen geführt: in beiden Köpfen (F2),
im abgelösten Dokument selbst (F9) und hier. Das ist keine Verschwendung,
sondern die Antwort auf drei verschiedene Zugriffswege — Werkzeug, geöffnetes
Einzeldokument, Überblick.

**Naheliegend und verworfen.** *Auf den Statusvermerk im Dokument vertrauen* —
notwendig, aber nicht hinreichend: Er beantwortet die Frage erst, wenn man das
richtige Dokument schon gefunden hat. *Das Verzeichnis aus den Köpfen erzeugen* —
richtig und empfehlenswert, sobald ein Werkzeug existiert; ohne Werkzeug ist ein
von Hand gepflegtes Verzeichnis besser als keines, und der Aufwand ist eine
Zeile je Entscheidung. *Auf die Verzeichnisliste des Dateisystems vertrauen* —
sie zeigt Nummer und Titel (F4), aber keinen Status.

**Woran man merkt, dass es verletzt ist.** Jemand setzt eine Festlegung um, die
seit einem halben Jahr abgelöst ist, und beruft sich dabei auf den Bestand.

**Tragwerk.** F1 trägt F3 und F5 — beide sind MADRs eigene Gestalt, nicht unsere
Erfindung; wer F1 aufgibt, muss sie neu begründen. F2, F9 und F10 gehören
zusammen: Sie führen dieselbe Ersetzung an drei Stellen, je für einen anderen
Zugriffsweg; fällt eine davon, bleibt die Ersetzung wahr, aber für einen Teil
der Leser unsichtbar. F3 ruht auf der Unveränderlichkeit des Inhalts — ohne sie
wäre es gleichgültig, was im Kopf steht. F4, F7 und F8 sind Handwerk und
unabhängig voneinander, jedes einzeln übernehmbar. F6 ist eine
Lesbarkeitsentscheidung; wer sie weglässt, verliert Bequemlichkeit, nicht
Substanz. **Wer nur eine Regel mitnimmt, nimmt F8** — die Vorlage trägt alle
anderen still mit.

## 5. Sicherheitsbetrachtung

Ein Dateiformat scheint sicherheitsneutral. Es ist es nicht, sobald die
Dokumente unveränderlich sind: Was einmal drinsteht, bleibt drin.

| Gefahr | Typischer Hergang | Gegenmaßnahme |
|---|---|---|
| **Personenbezogene Daten werden unveränderlich** | Die MADR-Felder `decision-makers`, `consulted`, `informed` laden dazu ein, Klarnamen einzutragen. In einem unveränderlichen, womöglich veröffentlichten Bestand steht der Name dann dauerhaft — mit einer Aussage darüber, wer wofür verantwortlich war | Rollen statt Namen eintragen (`Architektur`, `Betrieb`), oder Namen bewusst und mit Einverständnis. Bei einem Ein-Personen-Bestand unkritisch, bei einem Team nicht |
| **Geheimnis im Beispiel** | Ein Dokument begründet eine Entscheidung mit einem echten Schlüssel, Hostnamen oder Pfad. Später fällt es auf — der Text darf aber nicht mehr geändert werden | Vor der Annahme prüfen; Beispiele grundsätzlich mit erfundenen Werten. Die mitgelieferte Vorlage enthält deshalb ausschließlich Platzhalter |
| **Der Dateiname verrät die Architektur** | Sprechende Titel (F4) machen den Verzeichnisbaum lesbar — auch für Fremde. Ein Bestand, dessen Dateinamen „offener-lesezugriff" und „master-schluessel" heißen, ist eine Landkarte für einen Angreifer | Bewusste Entscheidung über die Veröffentlichung des Bestands; Titel beschreiben die Frage, nicht die Schwachstelle |
| **Der Kopf wird als Wahrheit gelesen** | Ein Werkzeug oder ein Assistenzsystem wertet nur die Kopffelder aus und meldet einen Bestand als aktuell, obwohl ein Dokument im Text etwas anderes sagt | F3 einhalten: Steht im Kopf nur Buchhaltung, kann er dem Text nicht widersprechen. Die Prüfung „stimmt der Kopf mit dem Text überein" entfällt damit |

**Wogegen das Format nicht hilft:** Es macht keine Entscheidung richtig, es
ersetzt keine Zugriffskontrolle, und es verhindert nicht, dass jemand ein
Dokument schreibt, das niemand liest.

## 6. Was das kostet

- **MADR ist geschwätzig.** Fünf Abschnitte für eine Entscheidung, die man in
  drei Sätzen sagen könnte. Bei kleinen Entscheidungen fühlt sich das Format wie
  Bürokratie an — und die Versuchung, sie deshalb gar nicht aufzuschreiben, ist
  real.
- **Der Kopf wird von Hand gepflegt.** Solange es kein Werkzeug gibt, sind die
  beiden Beziehungslisten reine Disziplin; ein vergessener Rückverweis fällt
  niemandem auf.
- **Die Doppelung der Optionen (F6) ist doppelte Arbeit.** Wer sie schludrig
  macht, hat eine Liste und einen Absatz, die einander widersprechen.
- **Vierstellige Nummern sehen bei zwölf Dokumenten albern aus.** Der Nutzen
  zeigt sich erst bei dreistelligen Beständen — bezahlt wird von Anfang an.
- **Die Vorlage muss gepflegt werden.** Ändert sich eine Formatregel, ist die
  Vorlage die erste Datei, die niemand anfasst.

## 7. Geltung und Grenzen

**Randbedingungen.** Markdown-Dateien in einem versionierten Verzeichnis, ein
Werkzeug, das YAML-Frontmatter wenigstens anzeigt, und die Bereitschaft, den Kopf
von Hand zu pflegen. Kein Server, keine Datenbank, kein ADR-Programm.

**Kipp-Bedingungen.** Die Antwort wird falsch, wenn (a) ein Werkzeug im Einsatz
ist, das ein eigenes Format erzwingt — dann gewinnt das Werkzeug, und dieses
Dossier beschreibt bestenfalls, welche Felder man darin nachbilden sollte; (b)
der Bestand so groß wird, dass Kopffelder von Hand nicht mehr stimmen — ab dort
braucht es einen Prüflauf, der Beziehungen und Verzeichnis gegeneinander hält;
(c) die Entscheidungen keine Optionen haben, weil sie von außen vorgegeben sind
— dann sind F5 und F6 leere Hüllen und eine schlichte Liste tut es auch.

**Nicht gelöst.** Dieses Dossier beschreibt die **Form** und sagt, an welchen
Stellen ein angenommenes Dokument noch verändert werden darf (F3, F9). Es sagt
nichts darüber, **wie eine Revision inhaltlich abläuft** — wann man revidiert,
statt zu präzisieren, wie vollständig das ersetzende Dokument die Frage neu
beantworten muss und was mit Teilen geschieht, die unverändert gelten. Es regelt
auch nicht, wer entscheiden darf, wann eine
Entscheidung dokumentierwürdig ist, und wie ein Bestand rückwirkend entsteht,
wenn jahrelang mündlich entschieden wurde.

## 8. Bewährung

Das Format läuft in einem privaten Projekt des Autors mit siebenunddreißig
Dokumenten, davon neun abgelöst. Es entstand nicht auf der grünen Wiese, sondern
durch **Umzug aus einer Sammeldatei**, in der Revisionen als Zusatzabsätze unter
der ursprünglichen Entscheidung gelandet waren. Der Umzug hat zwei Festlegungen
dieses Dossiers erzwungen: F2, weil beim Umzug drei Dokumente auftauchten, die
je zwei Vorgänger zusammenfassten — mit einem Einzelfeld hätte man einen davon
verschweigen müssen; und F8, weil die ersten von Hand nachgebauten Dokumente
jeweils ein anderes Kopffeld vermissen ließen.

**Anlage.** Die Vorlage liegt diesem Dossier bei — deutsch als
`examples/adr-template.de.md`, englisch als `examples/adr-template.en.md`. Sie
ist der eigentliche Gegenstand von F8: kopieren, umbenennen, ausfüllen. Ihr
Wortlaut, hier vollständig, damit dieses Dossier auch ohne die Anlage benutzbar
bleibt:

```markdown
---
status: entwurf            # entwurf | angenommen | abgeloest
date: JJJJ-MM-TT           # Tag der Entscheidung, nicht des Schreibens
decision-makers: []        # wer entschieden hat — Rollen statt Klarnamen
consulted: []              # wer gefragt wurde
informed: []               # wer es wissen muss
supersedes: []             # Nummern, die dieses Dokument ersetzt — IMMER Liste
superseded-by: []          # Nummern, die dieses Dokument ersetzt haben — IMMER Liste
---

# ADR-<nnnn> · <Titel: die Entscheidung, nicht das Thema>

## Kontext und Problem

<Woraus die Frage entsteht. Was war der Anlass, was ging schief, was stand an?
Nicht: „Dieses Dokument behandelt X." Sondern: das Problem in der Sprache
dessen, der es hatte.>

## Betrachtete Optionen

- **<Option A>** (gewählt)
- **<Option B>** — verworfen
- **<Option C>** — verworfen

## Entscheidung

**Entscheidung:** <Ein Satz im Indikativ, zitierbar, ohne Begründung.>

**Begründung:** <Der Grund, nicht die Herleitung.>

**Abwägung:** <Was für die Gegenseite sprach und warum es nicht gereicht hat.
Wer hier nichts schreiben kann, hat die Alternative nicht ernst genommen.>

## Folgen

<Positiv und negativ, beides ausgeschrieben. Die negativen sind der Grund,
warum dieser Abschnitt Pflicht ist — sie werden im Moment der Entscheidung
für unwichtig gehalten und sind ein Jahr später das Einzige, was zählt.>

## Verworfen und warum

**<Option B>:** <Der ausgeschriebene Grund. Dieser Abschnitt ist der Wert des
ganzen Bestands: Er verhindert, dass die verworfene Lösung wieder
vorgeschlagen wird.>

**<Option C>:** <…>
```

> **Beispiel** (aus demselben Projekt, durchgehend): Für die Gegenstände einer
> Inventarverwaltung wurde entschieden, dass ihre sichtbare Nummer ein
> Identitätsmerkmal ist. Das Dokument hieß
> `0011-sichtbare-nummer-identitaetsattribut.md`, sein Kopf trug
> `status: angenommen`, `date: 2026-08-07`, beide Beziehungslisten leer.
>
> Ein halbes Jahr später zeigte sich, dass Nummern nachträglich vergeben und
> korrigiert werden. Es entstand `0031-sichtbare-nummer-versioniert.md` mit
> `supersedes: [0011]`; in `0011` wurde `status: abgeloest` gesetzt,
> `superseded-by: [0031]` ergänzt und über die Überschrift ein Vermerk gesetzt,
> der auf den Nachfolger zeigt. Am Fließtext von `0011` wurde nichts geändert —
> er beschreibt weiterhin die Entscheidung von 2026-08-07, samt der Begründung,
> die damals richtig schien.
>
> Im Verzeichnis steht die Zeile von `0011` durchgestrichen mit dem Vermerk
> „↳ abgelöst von 0031", die Zeile von `0031` mit „ersetzt 0011". Beide
> Richtungen sind damit an drei Stellen geführt: in beiden Köpfen und im
> Verzeichnis.

Der Status dieses Dossiers bleibt **Entwurf**: Ein Bestand dieser Größe und ein
Betrieb von wenigen Monaten sind zu wenig für „Bewährt". Insbesondere die
Kipp-Bedingung (b) — Kopffelder, die von Hand nicht mehr stimmen — wurde noch
nicht erlebt.

## 9. Offene Fragen

**Q1** Sollte der Kopf ein Feld für die **Frage** tragen, die das Dokument
beantwortet — als eine Zeile, die ein Verzeichnis anzeigen kann? Das widerspricht
F3 („nur Buchhaltung"), löste aber ein echtes Problem: Titel sind kurz, und ein
Verzeichnis aus vierzig Titeln sagt weniger, als man hofft.

**Q2** Wie prüft man die Beziehungslisten automatisch, ohne ein ADR-Werkzeug
einzuführen? Gesucht ist der kleinste denkbare Prüflauf, der beide Richtungen
und das Verzeichnis gegeneinander hält.

**Q3** Ist die vierstellige Nummer (F4) übertrieben? Sie kostet in jedem Zitat
zwei Zeichen und rechtfertigt sich erst jenseits von hundert Dokumenten. Erfahrung
aus großen Beständen ist willkommen.

**Q4** Bei mehrsprachigen Teams: Ist ein Bestand in der Arbeitssprache mit
englischen Titeln ein tragfähiger Mittelweg, oder ist das der halb übersetzte
Bestand aus F7 unter anderem Namen?

## 10. Quellen & Verwandtes

- **MADR — Markdown Any Decision Records**, Fassung 4.0.0, mit Vorlagen und
  Feldbeschreibung: <https://adr.github.io/madr/> (abgerufen 2026-08-13).
- **Michael Nygard, „Documenting Architecture Decisions"** (2011), das
  ursprüngliche Format ohne YAML-Kopf, aus dem MADR hervorgegangen ist:
  <https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions>
  (abgerufen 2026-08-13).
- **Sammlung von Vorlagen und Praxisberichten zur Entscheidungsdokumentation:**
  <https://adr.github.io/> (abgerufen 2026-08-13).
- **Vertiefung, nicht Voraussetzung — und die Herkunft der Regeln in Abschnitt
  2:** Das Schwester-Dossier `immutable-decision-records`
  (<https://github.com/schlingensiepen/proposals/tree/main/immutable-decision-records>)
  beschreibt den
  Mechanismus, dem die hier beschriebenen Dokumente unterzogen werden — Ein
  Dokument je Entscheidung, Unveränderlichkeit nach der Annahme, vollständige
  Ersetzung statt Korrektur —, und zwar **losgelöst vom Format**: Er gilt für
  jeden Entscheidungslog, auch ohne MADR und ohne Architekturbezug. Dort steht
  auch, was dieses Dossier bewusst offenlässt: wie eine Revision inhaltlich
  abläuft, warum eine Ersetzung vollständig sein muss und was ein abgelöstes
  Dokument für den Bestand leistet. **Dieses Dossier ist ohne jenes vollständig
  benutzbar** — die Regeln, auf denen seine eigenen Festlegungen ruhen, stehen
  hier ausgeschrieben, samt Begründung.
