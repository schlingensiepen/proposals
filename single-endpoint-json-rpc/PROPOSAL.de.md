---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: single-endpoint-json-rpc
language: de (führend)
question: "Wie spreche ich mit dem Server, wenn ich nur billiges PHP-Hosting habe — kein URL-Rewriting, keine Bibliotheken, kein Framework?"
thesis: "Alle Aufrufe gehen als POST an einen einzigen Endpunkt; Methode, Parameter und Fehler stehen im Body, die Nachweise im Header — und Binärdaten bleiben draußen."
---

# Ein Endpunkt, ein Verb, ein Umschlag

**In einem Satz:** Die gesamte API ist ein einziger `POST`-Endpunkt, der
JSON-RPC 2.0 spricht; was nicht durch einen JSON-Umschlag passt — Bilder,
Archive, alles Binäre —, bekommt einen eigenen, ausdrücklich begründeten Weg.

**Kurzthese:** Auf billigem Shared Hosting kann man sich weder auf
URL-Rewriting noch auf installierbare Bibliotheken verlassen, und jedes
Geheimnis in einem Query-String landet im Zugriffslog. Ein einziger Endpunkt
mit einem etablierten RPC-Umschlag löst alle drei Probleme auf einmal, ohne
eine Zeile Fremdcode. Der Preis ist, dass HTTP-Infrastruktur nichts mehr über
die Aufrufe erfährt.

## 1. Problem

„Ich habe ein Hosting-Paket für ein paar Euro im Monat. Ich kann PHP-Dateien
hochladen, und das war es. Ob `mod_rewrite` aktiv ist, weiß ich nicht — auf dem
Windows-Paket desselben Anbieters gibt es das Modul gar nicht. Einen
Paketmanager habe ich nicht, also auch kein Framework. Und trotzdem soll da eine
API laufen, an die eine Web-App und ein Hintergrundprozess sprechen."

Daraus folgen drei Fragen, die zusammengehören und meist getrennt beantwortet
werden:

- **Wie sehen die Adressen aus?** Ohne Rewriting ist jede Adresse entweder eine
  Datei oder ein Verzeichnis. `/api/v1/kisten/7` ist beides nicht — der Server
  antwortet `404`, und zwar auf beiden Plattformen. Bleibt `/api/index.php/…`,
  das die Technologie in die Adresse schreibt, oder `/api/?p=…`, das die
  Fachlogik in den Query-String schreibt.
- **Wohin mit den Geheimnissen?** Ein Token im Query-String steht im
  Zugriffslog, in der Browser-Historie und in jedem `Referer`, den der Browser
  mitschickt. Das ist keine theoretische Gefahr, das ist der Normalbetrieb.
- **Wohin mit den Dateien?** Ein JSON-Umschlag trägt keine Bytes. Base64 kostet
  ein Drittel Volumen und zwingt den Server, die ganze Datei im Speicher zu
  halten — auf einem Paket mit knappem Speicherlimit und kurzem Zeitlimit ist
  das genau die Anfrage, die scheitert.

## 2. Das Konzept

Es gibt **eine** Adresse für die gesamte API: `POST <basis>/api/`. Der Webserver
löst sie über sein Standarddokument auf ein Skript auf — das kann jeder Server,
es ist kein Zusatzmodul. Der Body trägt einen **JSON-RPC-2.0-Umschlag**: den
Methodennamen, die Parameter als Objekt und eine `id`, die unverändert
zurückkommt. Methodennamen folgen dem Schema `bereich.tätigkeit`, sodass eine
alphabetische Liste der Methoden zugleich eine Gliederung ist. Die Antwort
enthält entweder `result` oder `error`; der HTTP-Status ist dabei **immer
`200`**, denn die Fachlage steht im Fehlerobjekt, nicht in der Statuszeile. Das
Fehlerobjekt hat drei Felder mit drei Aufgaben: eine **Zahl** mit der Semantik
der bekannten HTTP-Statuscodes, einen **maschinenlesbaren Typ** und einen
**Klartext für Menschen**. Nachweise — Sitzungs-Cookie, Maschinenschlüssel,
Verwaltungsschlüssel — reisen ausschließlich in **Headern**, nie im Body und nie
in der Adresse; den Sitzungsnachweis setzt der Server im Tausch gegen einen
Einlöse-Nachweis, der über das **Fragment** der Adresse hereinkommt — die eine
Stelle, die der Browser nicht an den Server schickt und die deshalb in keinem
Log steht.

Alles Binäre verlässt den Umschlag, und zwar in **drei** Ausprägungen:
**Hochladen** über einen eigenen Multipart-Endpunkt, der nur eine Datei annimmt
und eine Kennung zurückgibt; **Herunterladen** über eine unerratbare Adresse,
die der Webserver direkt ausliefert — ein zufällig benanntes Verzeichnis, in dem
die Datei ihren ursprünglichen Namen behält; und das **Bereitstellen** großer
Zusammenstellungen — Sicherungen, Exporte —, das über den Umschlag *beauftragt*
wird und erst nach Abschluss eine Adresse nach demselben Datei-Muster
zurückgibt. Der Umschlag kann die Bytes nicht tragen, aber er kann den **Auftrag**
tragen; damit bleibt die Fachlogik in der API und nur die Auslieferung außerhalb.

## 3. Begriffe

- **Endpunkt** — eine Adresse, an die Anfragen gehen. Hier: genau eine für die
  gesamte Fachlogik.
- **Umschlag** — die feste Hülle, in der jede Anfrage und jede Antwort steckt;
  hier das JSON-RPC-2.0-Objekt mit `jsonrpc`, `method`, `params`, `id`
  beziehungsweise `result`/`error`.
- **JSON-RPC 2.0** — ein 2010 verabschiedeter, sehr knapper Standard für
  entfernte Aufrufe über einen beliebigen Transportweg. Er definiert genau den
  Umschlag und eine Handvoll Protokollfehler; sonst nichts.
- **Standarddokument** — die Datei, die ein Webserver ausliefert, wenn eine
  Adresse auf ein Verzeichnis zeigt (`index.php`, `default.aspx`). Jeder Server
  kann das im Kern, ohne Zusatzmodul.
- **URL-Rewriting** — das Umschreiben von Adressen auf andere Dateien durch ein
  Servermodul. Auf günstigem Hosting nicht verlässlich vorhanden.
- **Nachweis** — eine Angabe, mit der ein Aufrufer belegt, wer oder was er ist:
  Sitzungs-Cookie, API-Schlüssel, Verwaltungsschlüssel.
- **Kennung** — eine unvorhersehbare Zeichenkette, die eine hochgeladene Datei
  benennt, bevor irgendjemand weiß, wozu sie gehört.
- **Fragment** — der Teil einer Adresse hinter dem `#`. Er wird vom Browser
  **nicht an den Server geschickt** und steht deshalb in keinem Zugriffslog und
  in keinem `Referer`; nur Code im Browser sieht ihn.
- **Einlöse-Nachweis** — ein einmalig vorzulegendes Geheimnis, das gegen einen
  dauerhaften Nachweis getauscht wird; typischerweise der Token aus einem
  Einladungslink.
- **Bereitstellung** — eine auf Auftrag zusammengestellte Datei, die vieles oder
  alles enthält: eine Datensicherung, ein Export. Abzugrenzen vom **Anhang**,
  der Teil der Fachdaten ist und mit ihnen entsteht.
- **Origin** — die Kombination aus Schema, Host und Port. Zwei Adressen mit
  gleicher Origin gelten dem Browser als dieselbe Herkunft; das entscheidet
  über Cookies und über die Frage, ob ein Aufruf einen Preflight braucht.

## 4. Festlegungen

### F1 · Eine Adresse, erreicht über das Standarddokument

**Was gilt.** Die gesamte Fachlogik hängt an genau einer Adresse, die auf ein
**Verzeichnis** zeigt (`/api/`) und über das Standarddokument des Servers auf
ein Skript aufgelöst wird. Kein Pfad hinter dem Verzeichnis, keine
Technologiekennung, kein Rewriting.

**Warum.** Ohne Rewriting ist eine Adresse entweder eine Datei oder ein
Verzeichnis — es gibt keine dritte Möglichkeit, und beide großen Webserver
antworten auf alles andere `404`. Das Standarddokument ist die einzige
Auflösungsregel, die jeder Server im Kern beherrscht; sie ist damit die
kleinste gemeinsame Voraussetzung, die man treffen kann. Dass die Adresse
nebenbei keine Technologie mehr nennt, ist ein Zusatznutzen, kein Grund: Ein
`index.php` in der Adresse macht jeden späteren Sprachwechsel zu einem Bruch
für alle Aufrufer.

**Naheliegend und verworfen.** *Pfade hinter der Skriptdatei*
(`/api/index.php/kisten/7`) — funktioniert auf manchen Konfigurationen und auf
anderen nicht; es hängt an einer Einstellung, die man auf fremdem Hosting weder
sieht noch ändert. *Fachlogik im Query-String* (`/api/?p=kisten.suchen`) — die
Adresse wird eindeutig, aber jeder Parameter landet im Zugriffslog, und die
Grenze zwischen „Steuerung" und „Geheimnis" hält niemand über Jahre durch.
*Rewriting voraussetzen und dokumentieren* — verlagert das Problem auf den
Betreiber und scheitert auf Plattformen, die das Modul gar nicht kennen.

**Woran man merkt, dass es verletzt ist.** Die API läuft auf dem einen
Hosting-Paket und antwortet auf dem anderen `404`, ohne dass sich am Code etwas
geändert hätte.

### F2 · Ein etablierter Umschlag, kein selbst erfundener

**Was gilt.** Der Umschlag ist JSON-RPC 2.0, unverändert: `jsonrpc`, `method`,
`params` als Objekt, `id`; in der Antwort `result` **oder** `error`. Kein
eigenes Format, keine Zusatzfelder auf der Umschlagebene.

**Warum.** Wer einen Endpunkt hat, braucht ein Feld, das sagt, was gemeint ist —
das ist unvermeidlich, sobald die Adresse nichts mehr aussagt. Diese Hülle
selbst zu erfinden kostet dieselbe Arbeit wie eine fertige zu übernehmen und
bringt den einen Nachteil, dass sie niemand kennt. JSON-RPC ist seit 2010
unverändert, passt auf zwei Seiten Text und verlangt keine Bibliothek: Der
Umschlag lässt sich in zwanzig Zeilen lesen und schreiben. Genau das ist auf
einem Hosting ohne Paketmanager das entscheidende Kriterium.

**Naheliegend und verworfen.** *Ein eigenes, „einfacheres" Format* — dieselben
Kosten, ohne dass Dritte es kennen; und die scheinbar überflüssigen Felder des
Standards holt man sich beim ersten Fehlerfall einzeln zurück. *GraphQL* —
lohnt bei vielen heterogenen Clients mit flexiblen Abfragen über tiefe Graphen;
bei einem flachen Modell mit wenigen bekannten Zugriffsmustern zahlt man eine
Bibliothek und eine Abfragesprache für nichts, und Dateiuploads sind dort ein
Sonderweg. *REST mit Ressourcen-Adressen* — die naheliegendste Wahl und an
F1 gescheitert: Ressourcen-Adressen sind ohne Rewriting nicht darstellbar.

**Woran man merkt, dass es verletzt ist.** Der Umschlag hat ein Feld bekommen,
das im Standard nicht vorgesehen ist — und ein zweiter Client wertet es nicht
aus.

### F3 · Keine Batch-Anfragen, keine Notifications

**Was gilt.** Ein Array im Body wird abgelehnt, ebenso eine Anfrage ohne `id`.
Beides erlaubt der Standard ausdrücklich; beides wird hier nicht benutzt.

**Warum.** Eine Batch-Anfrage hat kein Gesamtergebnis: Sie kann teilweise
gelingen, und damit braucht jede Fehlerbehandlung und jeder Test einen zweiten
Zweig, der bei einem einzelnen Aufruf gar nicht existiert. Eine Notification
(Anfrage ohne Antwort) verlagert die Frage „ist es angekommen?" in die
Zuversicht des Aufrufers. Beides kauft Rundreisen — und Rundreisen sind bei
einem System mit einem Betreiber und einer Handvoll Clients nicht das Problem,
das man hat.

**Naheliegend und verworfen.** *Batch für den Massenfall offenlassen* — dann
gibt es ihn, also wird er benutzt, also muss er getestet werden; eine
Fähigkeit, die man nicht ausschließt, ist eine, die man pflegt. *Notifications
für Protokollierung* — verlockend, aber ein verlorener Protokollaufruf fällt
niemandem auf, und genau deshalb ist er der falsche Ort zum Sparen.

**Woran man merkt, dass es verletzt ist.** Ein Fehlerbericht lautet „die Hälfte
ist durchgelaufen" — und niemand kann sagen, welche Hälfte.

### F4 · Der HTTP-Status ist `200`; die Fachlage steht im Umschlag

**Was gilt.** Solange die Anfrage überhaupt als JSON-RPC lesbar war, antwortet
der Server mit HTTP `200` — auch bei fachlichen Fehlern. Es gibt genau **zwei**
Ausnahmen: unlesbarer Body ergibt `400`, eine andere HTTP-Methode als `POST`
ergibt `405`. Beide tragen trotzdem ein vollständiges Fehlerobjekt im Body, mit
`id: null`, weil sich die `id` nicht lesen ließ.

**Warum.** Bei einem Endpunkt beschreibt der HTTP-Status nicht mehr das, was
passiert ist, sondern nur noch, ob der Umschlag ankam. Diese beiden Ebenen zu
vermischen — mal Status, mal Umschlag — zwingt jeden Client, zwei
Fehlerquellen zu behandeln und ihre Rangfolge zu kennen. Die zwei Ausnahmen
sind zulässig, weil dort **noch kein** Umschlag existierte: Wer kein lesbares
JSON geschickt hat, hat auch keine `id` geschickt.

Dass beide Ausnahmen dennoch ein Fehlerobjekt tragen, ist die eigentliche
Festlegung: **Ein Client bekommt nie einen leeren Body zu sehen.** Sonst
braucht er einen Sonderpfad für den Fall „Antwort ohne Inhalt", und dieser Pfad
wird selten geschrieben und noch seltener getestet.

**Naheliegend und verworfen.** *Fachliche Fehler auf HTTP-Statuscodes abbilden*
(`404` für „nicht gefunden") — sieht sauberer aus und ist eine Illusion: Der
Status beschreibt dann teils den Transport, teils die Fachlage, und bei einem
Zwischenserver, der selbst `404` erzeugen kann, sind die beiden nicht mehr
unterscheidbar. *Immer `200`, ausnahmslos* — bei unlesbarem Body müsste der
Server einen Erfolg melden, um einen Fehler zu transportieren; das ist eine
Lüge auf der Transportebene.

**Woran man merkt, dass es verletzt ist.** Ein Client fragt zuerst den Status
ab und wertet den Body nur „im Erfolgsfall" aus — und verliert damit
Fehlermeldungen.

### F5 · Der Fehler hat drei Felder mit drei Aufgaben

**Was gilt.** `error.code` trägt eine Zahl mit der **Semantik der
HTTP-Statuscodes** (400, 401, 404, 409, 410, 415, 500, 503).
`error.data.typ` benennt den konkreten Fall **maschinenlesbar**
(`nummer_vergeben`, `zustand_eingefroren`). `error.message` erklärt ihn im
**Klartext für Menschen**. Alle drei sind Pflicht.

**Warum.** Die drei Felder bedienen drei verschiedene Verbraucher, und jeder
Versuch, mit zweien auszukommen, schadet einem davon. Der Code erlaubt eine
grobe Behandlung ohne Kenntnis der Anwendung — „irgendetwas mit der Anfrage
war falsch" gegen „nicht angemeldet" —, und dass er die HTTP-Semantik
übernimmt, spart eine eigene Nummernkunde: Sie ist eingeführt, präzise und
jedem geläufig. Der Typ ist das, worauf ein Client tatsächlich verzweigt; ein
Klartext ist dafür ungeeignet, weil er sich ändert, sobald jemand die
Formulierung verbessert. Und der Klartext ist das, was ein Mensch liest — er
darf konkret sein, mit Zahlen und Namen darin.

**Naheliegend und verworfen.** *Nur Code und Klartext* — dann verzweigt der
Client auf Zeichenketten, und die erste Korrektur einer Formulierung bricht
ihn. *Nur Typ und Klartext* — dann braucht jeder Client die vollständige
Typenliste, bevor er überhaupt „das war ein Berechtigungsproblem" sagen kann.
*Eigene Nummern statt der HTTP-Semantik* — man erfindet eine Kunde nach, die
schon existiert, und muss sie dokumentieren.

**Woran man merkt, dass es verletzt ist.** Im Client steht ein Vergleich auf
einen Meldungstext.

### F6 · Protokollfehler und Fachfehler sind getrennte Nummernräume

**Was gilt.** Die vom Standard reservierten Codes (−32768 bis −32000) bleiben
ausschließlich dem Protokoll vorbehalten: unlesbares JSON, ungültiger Umschlag,
unbekannte Methode, `params` kein Objekt. Fachliche Fehler benutzen
ausschließlich die HTTP-artigen Codes. Kein Code kommt in beiden Rollen vor.

**Warum.** Die Grenze verläuft zwischen **Form** und **Inhalt**: „das ist keine
Parameterstruktur" ist etwas anderes als „das Feld `nummer` fehlt". Vermischt
man beides, kann ein Client nicht mehr entscheiden, ob er einen Fehler in
seinem *Code* oder in seinen *Daten* hat — und das ist der wichtigste
Unterschied beim Suchen. Praktisch heißt das: Ein fehlendes Fachfeld ergibt
`400`, nicht den Protokollfehler für ungültige Parameter.

**Naheliegend und verworfen.** *Den Protokollcode für „ungültige Parameter"
auch für fehlende Fachfelder benutzen* — er heißt schließlich so; genau daran
scheitert die Unterscheidung, denn dann bedeutet derselbe Code einmal „dein
Client ist kaputt" und einmal „der Benutzer hat ein Feld leer gelassen". *Einen
eigenen Protokollcode für interne Fehler verwenden* — der Standard sieht ihn
vor, aber er trägt in der Praxis keine brauchbare Meldung; ein interner Fehler
ist besser als fachlicher `500` mit Klartext aufgehoben.

**Woran man merkt, dass es verletzt ist.** Ein leer gelassenes Formularfeld
erzeugt denselben Fehlercode wie ein Programmierfehler im Client.

### F7 · Nachweise stehen im Header — nie im Body, nie in der Adresse

**Was gilt.** Jeder Nachweis reist in einem HTTP-Header. Kein Nachweis steht in
`params`, keiner im Query-String, keiner im Pfad. Es gibt drei Arten mit drei
Aufgaben:

| Art | belegt | wer setzt sie | Lebensdauer |
|---|---|---|---|
| **Sitzungsnachweis** (Cookie) | „dieser Mensch auf diesem Gerät" | der **Server**, per `Set-Cookie` | lang, einzeln entziehbar |
| **Maschinenschlüssel** (eigener Header) | „dieser Prozess darf diese Aufgabe" | bei der Einrichtung erzeugt, einmalig im Klartext ausgegeben | dauerhaft, einzeln entziehbar |
| **Verwaltungsschlüssel** (eigener Header) | „der Betreiber, im Einrichtungsvorgang" | beim Aufsetzen erzeugt | dauerhaft |

Der Sitzungsnachweis ist ein Cookie mit `HttpOnly`, `Secure` und
`SameSite=Lax`. Er wird bei jedem Besuch erneuert, damit er nicht mitten im
Gebrauch abläuft.

**Woher der Sitzungsnachweis kommt.** Er wird **eingetauscht**, nicht vergeben:
Der Aufrufer legt **einmalig** einen Einlöse-Nachweis vor — typischerweise den
Token aus einem Einladungslink —, und der Server antwortet mit `Set-Cookie`. Ab
da existiert der Einlöse-Nachweis für diesen Client nicht mehr.

**Wie dieser Einlöse-Nachweis in den Client kommt, ohne in ein Log zu
geraten:** über das **Fragment** der Adresse (`…/#einladung=<token>`), nicht
über den Pfad und nicht über den Query-String. **Der Browser schickt das
Fragment nicht an den Server.** Es steht damit in keinem Zugriffslog, in keinem
`Referer` und in keinem Zwischenserver — es ist die **einzige Stelle einer
Adresse, an der ein Geheimnis stehen darf.** Der Client liest es dort mit
JavaScript aus und schickt es **einmal** im Body eines Aufrufs; danach steht es
in keiner Adresse mehr.

**Was das von der Anwendung verlangt.** F7 beschreibt nicht nur einen
Transportweg — es **beauftragt** den Bauer. Wer eine Anwendung nach diesem
Muster baut, muss fünf Mechanismen bereitstellen; ohne sie ist die Festlegung
nicht umsetzbar:

1. **Einen Weg, einen Einlöse-Nachweis zu erzeugen und auszugeben** — die
   Einladung. Er ist unvorhersehbar, und die Anwendung weiß, wozu er berechtigt.
2. **Eine Methode, die ihn gegen einen Sitzungsnachweis tauscht** und dabei
   `Set-Cookie` setzt. Sie ist die einzige Stelle, an der ein Sitzungsnachweis
   entsteht.
3. **Eine Erneuerung des Sitzungsnachweises bei jedem Besuch**, damit er nicht
   mitten im Gebrauch abläuft.
4. **Eine Möglichkeit, einen einzelnen Nachweis zu entziehen** — ein Gerät, einen
   Maschinenschlüssel — ohne die übrigen zu treffen. Das setzt voraus, dass die
   Nachweise **unterscheidbar** sind: Was man nicht auseinanderhalten kann, kann
   man auch nicht einzeln sperren. Wer alle Geräte einer Person auf denselben
   Wert setzt, hat diesen Mechanismus verspielt, bevor er ihn braucht.
5. **Eine Ausgabe für Maschinenschlüssel, die den Klartext genau einmal zeigt.**
   Danach liegt in der Anwendung nur noch ein Streuwert; ein Schlüssel, der
   jederzeit wieder auslesbar ist, ist ein dauerhaft lesbares Geheimnis.

**Nicht vorgeschrieben ist die Politik**: wer einladen darf, wie lange eine
Einladung gilt, wie oft sie eingelöst werden kann, wie viele Geräte eine Person
haben darf, ob es Rollen gibt. Das entscheidet die Anwendung. **Die Mechanismen
oben sind dagegen Pflicht** — sie sind die Voraussetzung dafür, dass ein
Nachweis überhaupt entstehen, erneuert und zurückgenommen werden kann.

**Warum.** Adressen werden protokolliert — im Zugriffslog des Servers, in der
Browser-Historie, in Zwischenservern, in Fehlerberichten. Ein Geheimnis dort
ist nicht kompromittiert, sobald jemand angreift, sondern **von Anfang an**,
durch den Normalbetrieb. Der Body wird üblicherweise nicht protokolliert, wäre
aber die falsche Stelle aus einem anderen Grund: Nachweise gehören zur
Verbindung, nicht zum Aufruf — stünden sie in `params`, müsste jede einzelne
Methode sie mitschleppen und selbst prüfen, statt dass eine Stelle vor allen
Methoden entscheidet. Ein Endpunkt hat hier einen echten Vorteil: Es gibt genau
**eine** Stelle, an der die Prüfung stattfindet.

**`HttpOnly` ist dabei kein Detail:** Ein Cookie, das JavaScript nicht lesen
kann, kann auch nicht durch eine Schwachstelle im Client abfließen und nicht
versehentlich in eine Client-Protokollierung geraten. Genau deshalb ist der
Sitzungsnachweis ein Cookie und kein Wert, den der Client selbst verwaltet und
in einen Header schreibt — obwohl das auf den ersten Blick sauberer aussähe.

**Naheliegend und verworfen.** *Token als Parameter, „ist ja POST"* — der Body
ist heute nicht im Log und morgen vielleicht doch, sobald jemand zur Diagnose
mitschreibt; und die Prüfung wandert in die Methoden. *Token im Pfad, weil es
so schön RESTful aussieht* — dieselbe Protokollierung wie im Query-String, plus
`Referer`. *Den Einlöse-Token im Query-String des Einladungslinks* — der
naheliegendste Weg und der schlechteste: Er landet im Zugriffslog des Servers,
im `Referer` und in der Browser-Historie, und zwar bevor irgendjemand etwas
falsch gemacht hat. Das Fragment kostet nichts und vermeidet alle drei.
*Sitzungsnachweis als Wert im Speicher des Clients, per Header geschickt* —
verliert `HttpOnly` und damit den Schutz gegen Abfluss über den Client.
*Nachweis im ersten Aufruf, danach Zustand auf dem Server* — braucht
Sitzungsspeicher, den ein zustandsloses Skript nicht hat.

**Woran man merkt, dass es verletzt ist.** Im Zugriffslog steht eine Zeile, aus
der man sich anmelden kann.

### F8 · Binärdaten verlassen den Umschlag — in beide Richtungen

**Was gilt.** Bytes gehen nie durch den JSON-Umschlag. Hochgeladen wird über
einen **eigenen Multipart-Endpunkt** neben der API; heruntergeladen wird über
eine **unerratbare Adresse**, die der Webserver direkt ausliefert, ohne dass
Anwendungscode läuft.

**Warum.** JSON trägt keine Bytes. Base64 kostet ein Drittel Volumen — beim
Hochladen über eine Mobilverbindung ist das genau die Anfrage, die ins
Zeitlimit läuft. Schwerer wiegt der Speicher: Ein Base64-Feld muss vollständig
im Arbeitsspeicher stehen, bevor irgendetwas damit geschieht. Bei einer
Datenbanksicherung von einigen hundert Megabyte zwingt das den Prozess über
jedes Speicherlimit, das ein günstiges Paket setzt. Der Webserver dagegen
liefert eine Datei aus, ohne sie zu lesen.

**Der Preis ist ausdrücklich zu nennen:** Die unerratbare Adresse ist ein
**Nachweis in Form einer Zeichenkette**. Wer sie hat, kommt an die Datei — es
gibt keine Prüfung dahinter, keine Rücknahme, kein Ablaufdatum, das ein
statischer Webserver von sich aus durchsetzen könnte.

**Das ist dieselbe Art von Nachweis wie ein Token — mit einem Unterschied, der
zählt.** Ein Inhaber-Nachweis ist auch jeder Sitzungs- oder Maschinenschlüssel:
Wer ihn hat, darf. „Keine Rücknahme, kein Ablauf" trifft dort ebenso zu, sofern
niemand eine Sperrliste führt. Der Unterschied liegt **nicht in der Natur,
sondern im Ort**: Ein Token im Header steht in keinem Zugriffslog — ein Pfad
steht in jedem. Die Adresse dieser Datei erscheint im Serverlog, in der
Browser-Historie, in `Referer`-Headern und in jedem Zwischenserver, also genau
dort, wo F7 einen Nachweis ausdrücklich nicht haben will.

Das wird hier hingenommen, und die Begründung muss beide Hälften nennen: Der
**Schaden ist begrenzt** — die Adresse offenbart eine Datei, nicht das Konto und
nicht den Bestand —, und die Alternative kostet einen Skriptaufruf je
Vorschaubild. **Wo der Schaden nicht begrenzt ist, trägt diese Begründung
nicht**; dafür gilt F12. Vertretbar ist das Verfahren also für Inhalte, deren
Adresse nur der Berechtigte je zu sehen bekommt und deren Verlust einzeln
bleibt — und falsch für alles andere.

**Wie die Adresse gebaut wird, steht in F11.**

**Naheliegend und verworfen.** *Base64 im Umschlag, „ist doch nur ein Bild"* —
das Volumen ließe sich verschmerzen, der Speicherbedarf nicht; und die Grenze
zwischen „nur ein Bild" und „die ganze Datenbank" verschiebt sich mit der Zeit
nach oben. *Auslieferung über die API mit Berechtigungsprüfung* — dann läuft
für jedes Bild Anwendungscode, und jede Übersichtsseite mit zwanzig Vorschauen
startet zwanzig Skriptaufrufe. *Signierte Adressen mit Ablaufdatum* — die
richtige Lösung, sobald ein Server sie durchsetzen kann; ohne Rewriting und
ohne Anwendungscode im Auslieferungspfad ist sie nicht darstellbar.

**Woran man merkt, dass es verletzt ist.** Der Speicherverbrauch des Servers
hängt an der Größe der Nutzdaten statt an der Zahl der Anfragen.

### F9 · Die Dateiannahme trägt keine Fachlogik

**Was gilt.** Der Upload-Endpunkt nimmt **eine Datei** entgegen und gibt
**eine Kennung** zurück. Sonst nichts: kein Bezug zu einem Objekt, kein
Datenbankeintrag. Wozu die Datei gehört, sagt anschließend ein gewöhnlicher
RPC-Aufruf unter Angabe der Kennung.

**Warum.** Der Grund ist nicht Sauberkeit, sondern **Atomarität**. Ein Vorgang
mit drei Dateien entsteht als: dreimal hochladen, dann **ein** fachlicher
Aufruf mit drei Kennungen. Bricht der dritte Upload ab, ist nichts entstanden.
Der umgekehrte Weg — erst das Objekt anlegen, dann Dateien anhängen —
hinterlässt bei jedem Abbruch ein halbes Objekt; und wo die Historie
unveränderlich ist, lässt sich das nicht wegräumen. Der zweite Grund ist
Geschwindigkeit: Die Dateiannahme ist die langsamste Anfrage im System, und
alles, was man ihr aufbürdet, verlängert genau die, die am ehesten in ein
Zeitlimit läuft.

**Der Bereich, in den der Upload schreibt, wird vom Webserver direkt
ausgeliefert** (F8, F11). Damit ist er die gefährlichste Stelle des ganzen
Systems: Eine hochgeladene Skriptdatei würde dort beim Abruf **ausgeführt**, und
ein Verzeichnislisting machte aus „unerratbar" schlicht „aufzählbar". Drei
Riegel, in absteigender Verbindlichkeit — und die Reihenfolge ist wichtig, weil
auf günstigem Hosting die unteren beiden ausfallen können:

1. **Pflicht: den tatsächlichen Inhaltstyp prüfen**, nicht den mitgeschickten.
   Der mitgeschickte Typ kommt vom Aufrufer und ist damit kein Nachweis über
   irgendetwas. Diese Prüfung ist Anwendungscode und funktioniert deshalb
   **immer** — sie ist der einzige Riegel, auf den man sich verlassen kann.
2. **Pflicht: in jedes ausgelieferte Verzeichnis eine `index.html` und eine
   `index.php` legen**, beide leer oder mit einem Satz. Sie unterdrücken das
   Verzeichnislisting **ohne jede Serverkonfiguration**, weil jeder Webserver
   ein Standarddokument ausliefert, statt den Ordnerinhalt aufzuzählen — genau
   der Mechanismus aus F1, hier zum zweiten Mal genutzt. Zwei Dateien, weil die
   Server verschiedene Vorrangregeln haben und man nicht weiß, welche gilt.
   **Ohne diesen Riegel ist F8 wertlos:** Ein Listing zeigt alle Bezeichner auf
   einmal, und dann muss niemand mehr raten.
3. **Empfehlung: die Skriptausführung im Zielverzeichnis abschalten.** Das ist
   der wirksamste Riegel und zugleich der einzige, der eine **Serverkonfiguration
   voraussetzt** — eine `.htaccess`, die der Betreiber vielleicht gar nicht
   auswerten lässt, oder eine `web.config` auf der anderen Plattform. Auf einem
   Paket, dessen Konfiguration man weder sieht noch ändert, ist das eine
   Maßnahme, die man **trifft und nicht voraussetzt**. Wer sie zur Pflicht
   erklärt, baut seine Sicherheit auf eine Annahme, die er nicht prüfen kann.

**Die Aufteilung ist der Punkt.** Riegel 1 und 2 kosten nichts und wirken überall;
Riegel 3 wirkt am besten und kann still ausfallen. Deshalb darf keiner der drei
allein stehen — und deshalb darf Riegel 3 nicht der sein, auf den man sich
verlässt.

**Naheliegend und verworfen.** *Der Upload hängt die Datei gleich ein* — spart
einen Aufruf und kostet die Atomarität. *Aufräumen nie zugeordneter Dateien per
Zeitregel* — eine solche Regel löscht irgendwann, während jemand noch tippt;
verwaiste Dateien sind billiger als dieser Fehlerfall, und sie sind über einen
Abgleich von Verzeichnis gegen Datenbank auffindbar.

**Woran man merkt, dass es verletzt ist.** Nach einem abgebrochenen Upload
steht ein halber Vorgang in den Daten.

### F10 · Statische Auslieferung und API sind nach außen eine Origin

**Was gilt.** Frontend-Dateien und API liegen unter derselben Origin, gelöst
über **Verzeichnisse** unterhalb derselben Basis: die Anwendung an der Wurzel,
die API unter `/api/`, die Binärdaten unter einem eigenen Verzeichnis.

**Warum.** Zwei Origins bedeuten sofort drei zusätzliche Baustellen:
CORS-Header, ein Preflight vor jedem Aufruf mit Inhaltstyp `application/json`,
und Cookies, die als Drittanbieter-Cookies gelten und je nach
Browsereinstellung verschwinden. Keine davon ist unlösbar; alle drei sind
umsonst, wenn man sie vermeiden kann. Und sie zu vermeiden kostet auf dieser
Plattform nichts, weil die statischen Dateien ohnehin von demselben Server
kommen. Der Cookie-Nachweis aus F7 funktioniert nur unter dieser Bedingung
ohne Sonderbehandlung.

**Naheliegend und verworfen.** *API auf einer eigenen Subdomain* — ordentlich
und üblich, kostet hier aber genau die drei Baustellen oben für keinen
Gegenwert; eine eigene Subdomain lohnt, wenn API und Frontend getrennt
skalieren oder von verschiedenen Teams betrieben werden. *Statische Dateien von
einem CDN, API vom eigenen Server* — für große, öffentlich lesbare Inhalte
sinnvoll und dann über gemeinsame Origin mit Pfad-Routing zu lösen; hier liegt
alles hinter einem Nachweis, sodass ein CDN nichts ausliefern dürfte.

**Woran man merkt, dass es verletzt ist.** Vor jedem Aufruf steht ein
`OPTIONS` im Log — oder das Sitzungs-Cookie verschwindet in einem Browser mit
strengen Voreinstellungen.

### F11 · Der Zufall steckt im Verzeichnis, nicht im Dateinamen

> Diese Festlegung beschreibt den Mechanismus, den F8 und F9 benutzen. Sie steht
> hier hinten, weil Nummern nachträglich nicht neu vergeben werden; gelesen
> gehört sie zwischen F8 und F9.

**Was gilt.** Für Binärdaten gibt es einen **beschreibbaren Bereich, den der
Webserver als ganz gewöhnlichen Dateibereich ausliefert** — kein Anwendungscode,
keine Sonderbehandlung. Darin bekommt **jede Datei ein eigenes
Unterverzeichnis**, dessen Name ein **kryptographisch zufälliger Bezeichner**
ist (eine UUIDv4 oder 128 Bit aus einem sicheren Zufallsgenerator, hexadezimal).
Die Datei darin **behält ihren Namen aus dem Upload**. Die Adresse lautet damit:

```
<basis>/<bereich>/<zufalls-guid>/<originalname>
```

**Warum.** Der Zufall steckt im Verzeichnis und nicht im Dateinamen — aus drei
Gründen, die zusammen die Bauweise erzwingen:

- **Irgendwo muss der Zufall stecken**, sonst ist die Adresse ableitbar und F8
  hinfällig. Im Verzeichnis ist Platz dafür, weil dort sonst nichts
  untergebracht werden muss.
- **Der Dateiname bleibt sprechend.** Wer die Datei speichert, bekommt
  `rechnung-2026.pdf` und nicht `9f2c4a1e…`. Ein Zufallsname bräuchte einen
  `Content-Disposition`-Header, um das zu heilen — und den setzt nur, wer
  Anwendungscode im Auslieferungspfad hat. Genau den schließt F8 aus. **Die
  Bauweise ist also nicht Geschmack, sondern die Folge von F8.**
- **Namenskollisionen sind ausgeschlossen**, ohne dass irgendwer umbenennt: Zwei
  Dateien gleichen Namens liegen in verschiedenen Verzeichnissen.

**Der Bezeichner muss aus einem kryptographischen Zufallsgenerator kommen.** Ein
Zähler, ein Zeitstempel oder die übliche schwache Zufallsfunktion einer Sprache
macht die Adresse vorhersagbar — und damit F8 wertlos, ohne dass man es merkt.

**Naheliegend und verworfen.** *Zufälliger Dateiname in einem gemeinsamen
Verzeichnis* — spart ein Verzeichnis, kostet den sprechenden Namen; außerdem ist
ein Ordner mit zehntausend Dateien auf manchen Dateisystemen spürbar langsam.
*Gliederung nach Datum oder Objekt* (`/2026/08/eintrag-7/`) — macht die Adresse
ableitbar; wer ein Objekt kennt, errät seine Dateien. *Inhaltshash als
Bezeichner* — verlockend, weil es doppelte Dateien zusammenlegt, aber der Hash
ist berechenbar: Wer denselben Inhalt besitzt, kennt die Adresse. *Beide
Fassungen einer Datei (Original und Vorschau) in ein Verzeichnis* — verlockend,
weil der Ordner dann „auch ohne Datenbank etwas wert" wäre. Ist er nicht: Die
Ordner heißen nach Zufallswerten; ohne Datenbank weiß niemand, wozu sie gehören,
ob nun eine Datei darin liegt oder zwei.

**Woran man merkt, dass es verletzt ist.** Aus einer bekannten Datei-Adresse
lässt sich eine zweite erraten — oder ein Abruf ohne Dateinamen zeigt den
Verzeichnisinhalt.

### F12 · Große Zusammenstellungen: beauftragen, abfragen, abholen, beenden

**Was gilt.** Muss die Anwendung größere Datenbestände **zusammenstellen** —
eine Sicherung, ein Export —, geschieht das nicht über einen festen Pfad,
sondern über Methoden im Endpunkt:

| Methode | Aufgabe |
|---|---|
| `…beauftragen` | gibt die Erstellung in Auftrag. Antwortet mit dem Download-Pfad, wenn sofort erzeugt wird — sonst mit einer **Auftragskennung** |
| `…status` | nur bei nebenläufiger Erstellung. Liefert den Zustand und **erst nach Abschluss** den Download-Pfad, gebildet nach dem üblichen Datei-Muster (F11) |
| `…beenden` | beendet die Bereitstellung: Datei und Pfad verschwinden. **Bei sicherheitsrelevanten Daten ist diese Methode Pflicht**, nicht Kür |

**Zusätzlich kann für kritische Daten eine Frist definiert werden**, nach der
eine Bereitstellung auch ohne Beendigung verfällt.

**Warum.** Eine Bereitstellung nimmt denselben Weg wie ein Anhang (F8, F11) und
sieht deshalb aus wie einer — aber der Schaden ist ein anderer: Die Adresse
eines Anhangs offenbart **einen Anhang**, die einer Sicherung **den gesamten
Bestand**. Damit trägt die Rechtfertigung aus F8 — „der Schaden ist begrenzt" —
hier gerade nicht, und ohne eigene Festlegung erbte die Sicherung stillschweigend
eine Begründung, die für sie nie galt. Der Umschlag kann die Bytes nicht tragen,
aber er kann den **Auftrag** tragen; deshalb bleibt die Steuerung — anfordern,
nachfragen, beenden — in der API, und nur die Auslieferung liegt außerhalb. Die
drei Absätze darunter begründen die drei Teile einzeln.

**Warum die Adresse erst nach Abschluss entsteht.** Gäbe es sie vorher, läge
unter einer gültigen Adresse eine halbe Datei — und ein Abrufer kann eine
unvollständige Sicherung nicht von einer vollständigen unterscheiden. Ein
Zustand ist eine Auskunft, ein Pfad ist ein Versprechen; solange die
Zusammenstellung läuft, darf es nur die Auskunft geben. Das gilt unabhängig
davon, ob nebenläufig gearbeitet wird — es ist bloß dort unvermeidlich.

**Warum das Beenden eine eigene Methode ist und nicht automatisch geschieht.**
Der Server weiß nicht, wann ein Abruf fertig ist; ein abgebrochener Download
sieht für ihn aus wie ein erfolgreicher. Der **Abrufende** weiß es. Deshalb
meldet er es, und derselbe Aufruf räumt auf. Damit hängt die Lebensdauer an
einem Ereignis, das tatsächlich beobachtet wird, statt an einer Uhr, die niemand
stellt.

**Warum die Frist trotzdem dazugehört — und wie sie ohne zeitgesteuerten Prozess
durchsetzbar ist.** Bleibt die Beendigung aus, weil ein Abruf abbricht oder ein
Prozess stirbt, liegt eine vollständige Kopie des Bestands herum. Für kritische
Daten ist das nicht hinnehmbar. Ein günstiges Hosting-Paket hat aber keinen
verlässlichen Zeitgeber. Die Auflösung ist **träge Vollstreckung**: Jeder Aufruf
einer der drei Methoden räumt abgelaufene Bereitstellungen mit weg. Das kostet
nichts und braucht nichts — es hat die Lücke, dass nichts aufgeräumt wird,
solange niemand etwas beauftragt (Q4).

**Naheliegend und verworfen.** *Die Bereitstellung wie einen Anhang behandeln,
also unter einem dauerhaften Pfad ablegen* — bequem und die eigentliche Gefahr;
eine Sicherung, die dauerhaft unter einer Adresse liegt, ist ein Datenabfluss,
der nur darauf wartet, dass die Adresse einmal irgendwohin gerät. *Die
Zusammenstellung durch den Umschlag streamen* — erzeugt genau den Speicherbedarf,
den F8 vermeidet. *Nur eine Frist, ohne Beenden-Methode* — dann liegt die Datei
bis zum Ablauf herum, obwohl sie längst abgeholt ist; die Frist ist die
Rückfallebene, nicht der Regelfall. *Nur beenden, ohne Frist, auch bei kritischen
Daten* — verlässt sich darauf, dass die Bestätigung immer kommt. *Den Pfad schon
beim Beauftragen zurückgeben und den Abrufer pollen lassen, ob die Datei schon
da ist* — verlagert die Unterscheidung „fehlt noch" gegen „ist kaputt" auf den
Client, der sie nicht treffen kann.

**Woran man merkt, dass es verletzt ist.** Im Bereitstellungsverzeichnis liegen
Dateien von letzter Woche — oder ein Abruf liefert eine Datei, die sich nicht
öffnen lässt.

**Tragwerk.** **F1 trägt alles.** Fällt die Randbedingung „kein Rewriting", ist
F1 hinfällig, und mit ihm der Hauptgrund für F2 — dann sind Ressourcen-Adressen
möglich, und REST ist eine ernsthafte Alternative. F3 bis F6 beschreiben den
Umschlag und stehen und fallen mit F2; untereinander sind sie unabhängig, F5
und F6 lassen sich einzeln in jede andere API übernehmen. F7 ist von allem
anderen unabhängig und die Festlegung mit der größten Reichweite.

**F8 bis F12 sind der Preis von F2**, nicht seine Ergänzung: Sie existieren
ausschließlich, weil der Umschlag keine Bytes trägt. Untereinander hängen sie
eng zusammen, und die Kette ist einseitig:

- **F11 ist der Mechanismus**, den F8 nur behauptet. Ohne F11 ist „unerratbare
  Adresse" eine Absichtserklärung.
- **F8 ruht auf F11**, nicht umgekehrt — und F11 ruht auf F8, was den Zufall im
  Verzeichnis erzwingt: Ohne Anwendungscode im Auslieferungspfad gibt es keinen
  `Content-Disposition`-Header, also muss der Dateiname sprechend bleiben.
- **F9 ruht auf beiden.** Sein zweiter Riegel (`index.html`/`index.php`) ist
  keine Zugabe, sondern die Bedingung dafür, dass F8 überhaupt gilt: Ein
  Verzeichnislisting macht aus „unerratbar" „aufzählbar", und dann ist der
  Zufall aus F11 wertlos.
- **F12 ist die Grenze von F8** und zugleich das einzige Muster hier, das den
  Umschlag und den Dateiweg **zusammenführt**: Der Auftrag geht durch den
  Umschlag, die Bytes nicht. Fällt F12 weg, erbt die Datensicherung die
  Rechtfertigung des Anhangs, die für sie nie galt — der gefährlichste einzelne
  Ausfall in diesem Dossier.

F10 steht für sich, macht aber F7 erst bequem. **Wer nur eine Regel mitnimmt,
nimmt F7.**

## 5. Sicherheitsbetrachtung

| Gefahr | Typischer Hergang | Gegenmaßnahme |
|---|---|---|
| **Geheimnis im Protokoll** | Ein Token steht im Query-String. Es landet im Zugriffslog, in der Browser-Historie, im `Referer` an fremde Server und in jedem Fehlerbericht, den jemand weiterleitet | F7: Nachweise nur in Headern. Diese Gefahr entsteht nicht durch einen Angriff, sondern durch den Normalbetrieb — sie ist deshalb die einzige, die man durch Bauweise vollständig ausschließen kann |
| **Hochgeladenes Skript wird ausgeführt** | Eine Datei mit Skript-Endung landet in einem Verzeichnis, das der Webserver direkt ausliefert, und wird beim Abruf ausgeführt | Drei Riegel (F9), nach Verbindlichkeit gestuft: **Pflicht** der tatsächliche Inhaltstyp beim Annehmen (Anwendungscode, wirkt immer), **Pflicht** die Standarddokumente gegen das Listing, **Empfehlung** die abgeschaltete Skriptausführung — sie ist die wirksamste und zugleich die einzige, die eine Serverkonfiguration braucht und deshalb still ausfallen kann |
| **Unerratbare Adresse ist keine Berechtigung** | Eine Datei-Adresse wird weitergeleitet, steht in einer Browser-Historie auf einem geteilten Rechner oder in einem Backup. Wer sie hat, kommt an die Datei | Bewusst hinnehmen und begrenzen (F8): nur für Inhalte, deren Verlust **einzeln** bleibt. Der Unterschied zu einem Token liegt nicht in der Natur — beide sind Inhaber-Nachweise —, sondern im Ort: Ein Pfad wird protokolliert, ein Header nicht |
| **Verzeichnislisting hebt die Unerratbarkeit auf** | Ein Abruf des Verzeichnisses ohne Dateinamen zeigt alle Bezeichner auf einmal. Aus „unerratbar" wird „aufzählbar", und der Zufall aus F11 ist wertlos | `index.html` **und** `index.php` in jedem ausgelieferten Verzeichnis (F9, Riegel 2). Kostet nichts und braucht **keine Serverkonfiguration** — derselbe Standarddokument-Mechanismus wie in F1. Sich stattdessen auf eine Servereinstellung zu verlassen, die man auf fremdem Hosting nicht prüfen kann, wäre der Fehler |
| **Die Bereitstellung erbt die Rechtfertigung des Anhangs** | Eine Datensicherung wird wie ein Anhang unter einer dauerhaften Adresse abgelegt. Die Begründung „der Schaden ist begrenzt" wird stillschweigend mitgenommen — für eine Datei, die den gesamten Bestand enthält | F12: beauftragen, abfragen, abholen, **beenden**. Die Beendigung ist eine eigene Methode und bei sicherheitsrelevanten Daten Pflicht; für kritische Daten zusätzlich eine Frist, träge vollstreckt |
| **Halbe Datei unter gültiger Adresse** | Die Adresse wird ausgegeben, während die Zusammenstellung noch läuft. Ein Abrufer bekommt eine unvollständige Sicherung und kann sie nicht von einer vollständigen unterscheiden | F12: Die Adresse entsteht **erst nach Abschluss**. Vorher gibt es nur einen Zustand, keinen Pfad |
| **Angriffe sehen aus wie Normalbetrieb** | Alles ist `200 POST /api/`. Ein Zwischenserver, ein Ratenbegrenzer oder eine Auswertung des Zugriffslogs kann Anmeldeversuche nicht von Suchanfragen unterscheiden | Bewusster Preis von F4. Wer Erkennung auf dieser Ebene braucht, muss sie in der Anwendung führen — ein eigenes Protokoll mit Methodennamen und Ergebnis. Das ist Mehraufwand, den F4 verursacht |
| **Auskunftsfreudige Fehlermeldungen** | Der Klartext eines internen Fehlers nennt Pfade, Tabellennamen, Bibliotheksversionen | Vertretbar bei einem System mit einem Betreiber, wo die Diagnosefähigkeit schwerer wiegt. Bei mehreren Mandanten oder öffentlichem Zugang **nicht**: Dort trägt `error.message` bei internen Fehlern nur eine allgemeine Auskunft und die Einzelheit wandert ins Serverprotokoll |
| **Ein Endpunkt, eine Prüfstelle** | Die Rechteprüfung sitzt an einer Stelle vor allen Methoden. Ein Fehler dort trifft **alles** | Das ist zugleich der Vorteil: Eine Stelle ist prüfbar, zwanzig sind es nicht. Die Prüfung gehört **vor** jede Auflösung von Bezeichnern, sonst verrät schon die Fehlermeldung, ob ein Objekt existiert |

**Wogegen das Konzept nicht hilft:** Es sagt nichts darüber, **wer** was darf.
Es verlangt die Mechanismen, mit denen ein Nachweis entsteht, erneuert und
einzeln zurückgenommen wird (F7), und legt fest, wo er reist und dass er an
einer Stelle geprüft wird — welche Politik darauf läuft, entscheidet die
Anwendung. Es schützt nicht vor fachlichen Fehlern in den Methoden und ersetzt
keine Eingabevalidierung.

## 6. Was das kostet

- **Die Infrastruktur sieht nichts mehr.** Im Zugriffslog steht `200 POST
  /api/`, immer. Statuscode-basiertes Monitoring, Ratenbegrenzung nach
  Fehlerrate, Auswertungen nach Endpunkt — all das funktioniert nicht mehr.
- **HTTP-Caching entfällt vollständig.** Jeder Aufruf ist ein `POST` auf
  dieselbe Adresse. Wo Antworten cachebar wären, muss man es selbst bauen.
- **Debuggen wird unbequemer.** Kein Aufruf lässt sich in eine Adresszeile
  tippen; jede Diagnose braucht ein Werkzeug, das einen Body schickt.
- **Der Umschlag ist weniger bekannt als REST.** Jeder neue Client-Autor liest
  erst zwei Seiten Standard, bevor er den ersten Aufruf macht.
- **Aus einem Endpunkt sind vier Eingänge geworden.** Die These heißt „ein
  Endpunkt", tatsächlich gibt es die API, den Upload, den Binärabruf und die
  Bereitstellung. Vier Dinge mit vier Sicherheitsbetrachtungen — F8 bis F12 sind
  kein Detail, sondern **fünf der zwölf Festlegungen und der größere Teil der
  Angriffsfläche**. Wer dieses Konzept übernimmt, übernimmt vor allem diese
  Ausnahmen.
- **Verwaiste Dateien.** F9 erzeugt sie zwangsläufig; sie zu finden ist ein
  Verzeichnisabgleich, sie wegzuräumen ein Handgriff, an den niemand denkt.

## 7. Geltung und Grenzen

**Randbedingungen.** Ein Webserver, der Skripte ausführt und ein
Standarddokument auflöst; ein `POST`-Body, den das Skript lesen kann; ein
Client, der Header setzen kann. Kein Rewriting, keine Bibliothek, kein
Framework, kein Paketmanager. Statische Dateien und API kommen von derselben
Origin. Wenige, bekannte Clients und ein Betreiber.

**Kipp-Bedingungen.** Die Antwort wird falsch, wenn:

- **viele heterogene Clients** flexible Abfragen über tiefe Graphen brauchen —
  dann rechnet sich GraphQL, und die Bibliothek ist ihr Geld wert;
- **HTTP-Caching gebraucht wird**, weil große, unveränderliche Inhalte über die
  API gehen — dann sind Ressourcen-Adressen mit `GET` die richtige Antwort, und
  F1 muss durch eine Umgebung mit Rewriting abgelöst werden;
- **Infrastruktur die Semantik sehen muss** — ein API-Gateway, eine Web
  Application Firewall, ein Ratenbegrenzer je Endpunkt. Sie alle arbeiten auf
  Methode, Pfad und Status; hinter einem Endpunkt sind sie blind;
- **Antworten gestreamt** werden müssen oder lange laufen. Der Umschlag setzt
  eine Antwort in einem Stück voraus.

**Nicht gelöst.** Beim Zugriff ist sauber zu trennen, was dieses Konzept
**verlangt** und was es **offenlässt**. Verlangt sind die fünf Mechanismen aus
F7 — Einladung erzeugen, gegen einen Sitzungsnachweis tauschen, erneuern,
einzeln entziehen, Maschinenschlüssel einmalig ausgeben. Sie sind ein Auftrag an
den Bauer, keine Auslassung. **Offen bleibt die Politik**: wer einladen darf,
wie lange eine Einladung gilt, wie oft sie eingelöst werden kann, wie viele
Geräte eine Person haben darf, ob es Rollen gibt und was bei Verlust geschieht.
Das ist eine Entscheidung der Anwendung und ein Gegenstand für sich. Es sagt nichts
über die **Versionierung** der Schnittstelle (siehe Q1), nichts über
**Ereignisse in Gegenrichtung** (Push, Streaming, lange Anfragen), und nichts
darüber, **wie das Frontend ausgeliefert** wird — F10 legt nur fest, dass es
dieselbe Origin ist.

## 8. Bewährung

**Ein vollständiger Vorgang** — ein Eintrag mit drei Bildern in einer
Inventarverwaltung, von Anfang bis Ende. *Beispiel; Namen und Werte sind
erfunden.*

Zuerst dreimal die Dateiannahme. Sie kennt weder Eintrag noch Zweck:

```
POST /api/upload.php
Content-Type: multipart/form-data; boundary=…
Cookie: sitzung=…

  (Feld "datei": die Bilddatei)
```

```json
{"kennung": "9f2c4a1e-7b03-4d55-9c18-2e6a0b8f1d34"}
```

Erst der fachliche Aufruf stellt den Zusammenhang her — **ein** Aufruf, drei
Kennungen, ein unteilbares Ergebnis:

```
POST /api/
Content-Type: application/json
Cookie: sitzung=…
```

```json
{"jsonrpc": "2.0",
 "method": "zustaende.anlegen",
 "params": {"kiste": 7,
            "text": "Weihnachtsschmuck, oberes Regal",
            "fotos": ["9f2c4a1e-…", "a71b0c93-…", "c04e58d7-…"]},
 "id": 1}
```

```json
{"jsonrpc": "2.0", "result": {"zustand": 42}, "id": 1}
```

Bricht der dritte Upload ab, ist **nichts** entstanden — es gibt keinen halben
Eintrag, nur drei verwaiste Dateien ohne Datenbankzeile (F9).

Ein fachlicher Fehler sieht so aus. HTTP-Status ist `200`; die drei Felder
tragen drei verschiedene Auskünfte:

```json
{"jsonrpc": "2.0",
 "error": {"code": 409,
           "message": "Nummer „12\" ist bereits an Eintrag #7 vergeben",
           "data": {"typ": "nummer_vergeben"}},
 "id": 2}
```

Ein Protokollfehler dagegen benutzt den reservierten Nummernraum — der Client
erfährt damit, dass **sein Code** falsch ist und nicht seine Daten (F6):

```json
{"jsonrpc": "2.0",
 "error": {"code": -32601, "message": "Unbekannte Methode: kisten.suche"},
 "id": 3}
```

Die Bilder holt der Browser **nicht** über die API, sondern direkt vom
Webserver — ein zufällig benanntes Verzeichnis, in dem die Datei ihren
ursprünglichen Namen behalten hat (F8, F11):

```
GET /bilder/9f2c4a1e-7b03-4d55-9c18-2e6a0b8f1d34/regal-oben.jpg
```

**Und derselbe Weg für eine Datensicherung — mit den drei Zusätzen aus F12.**
*Beispiel; Werte erfunden.* Ein Sicherungsprozess beauftragt die
Zusammenstellung. Weil sie Minuten dauert, kommt kein Pfad zurück, sondern eine
Auftragskennung:

```json
{"jsonrpc": "2.0", "method": "sicherung.beauftragen", "params": {}, "id": 10}
```

```json
{"jsonrpc": "2.0", "result": {"auftrag": "b23f9c01-…", "zustand": "laeuft"}, "id": 10}
```

Solange die Zusammenstellung läuft, gibt es **keinen Pfad** — nur einen Zustand.
Erst der Abschluss erzeugt ihn, nach demselben Muster wie ein Anhang:

```json
{"jsonrpc": "2.0", "method": "sicherung.status", "params": {"auftrag": "b23f9c01-…"}, "id": 11}
```

```json
{"jsonrpc": "2.0",
 "result": {"zustand": "fertig",
            "pfad": "/ausleitung/7d4e2a95-…/inventar-2026-08-14.sqlite",
            "groesse": 41582592},
 "id": 11}
```

Der Prozess holt die Datei direkt vom Webserver — und **meldet danach den
geglückten Abruf**. Derselbe Aufruf beendet die Bereitstellung; Datei und Pfad
verschwinden:

```json
{"jsonrpc": "2.0", "method": "sicherung.beenden", "params": {"auftrag": "b23f9c01-…"}, "id": 12}
```

Bricht der Abruf ab, bleibt die Datei liegen — und wird beim nächsten
`sicherung.beauftragen` mit weggeräumt, sofern die Frist abgelaufen ist (F12,
träge Vollstreckung). Das ist der Unterschied zum Bild oben: Dessen Adresse darf
bleiben, weil sie ein Bild offenbart. Diese offenbart den ganzen Bestand.

**Referenz-Implementierung: entfällt, weil noch keine existiert (Status:
Entwurf).** Das Konzept ist vollständig spezifiziert und in Vertragstests
beschrieben, aber die laufende Anwendung, aus der es stammt, steht auf einem
älteren Stand und bildet es noch nicht ab. Die Aussagen dieses Dossiers sind
damit **durchdacht, nicht erprobt** — insbesondere die Behauptung, dass F9 die
Atomarität tatsächlich herstellt, ist bisher nur ein Argument.

## 9. Offene Fragen

**Q1 · Versionierung.** Ein Endpunkt hat keinen Platz für eine Version, und der
Umschlag sieht keine vor. Denkbar sind ein Suffix am Methodennamen, ein eigener
Header oder ein zweites Verzeichnis daneben. Bisher ist die Frage nicht
beantwortet, weil es genau einen Client-Autor gibt — was keine Antwort ist,
sondern ein Aufschub.

**Q2 · Was passiert, wenn doch ein Zwischenserver davorsteht?** F4 nimmt an,
dass der Server den Status selbst bestimmt. Ein Reverse Proxy, ein
Lastverteiler oder ein Ausfall erzeugt eigene Statuscodes mit eigenen
Fehlerseiten — dann bekommt der Client doch einen Body, der kein Umschlag ist.
Braucht F4 eine Regel für diesen Fall, oder ist „was kein JSON ist, ist ein
Transportfehler" ausreichend?

**Q3 · Wo genau liegt die Grenze beim Klartext interner Fehler?** „Ein
Betreiber, also darf die Ausnahme im Klartext heraus" ist eine bequeme Regel.
Ab wann kippt sie — beim zweiten Benutzer, beim ersten fremden Client, beim
ersten öffentlich erreichbaren Formular?

**Q4 · Träge Vollstreckung — reicht das?** F12 setzt eine Frist durch, indem
jeder Aufruf der Bereitstellungsmethoden abgelaufene Dateien mit wegräumt. Das
kommt ohne zeitgesteuerten Prozess aus, hat aber eine Lücke: Wird lange nichts
beauftragt, wird auch nichts aufgeräumt — und genau dann liegt eine vollständige
Kopie am längsten da. Ist ein zusätzlicher Aufräumschritt an anderer Stelle
nötig, und wo?

**Q5 · Trägt Riegel 2 wirklich überall?** F9 verlässt sich darauf, dass eine
`index.html` das Verzeichnislisting unterdrückt, weil jeder Webserver ein
Standarddokument ausliefert. Für die verbreiteten Server gilt das; ob es
**überall** gilt — auch hinter Objektspeichern, CDNs und Auslieferern, die
Verzeichnisse anders behandeln —, ist ungeprüft. Der Riegel ist als Pflicht
formuliert, seine Allgemeingültigkeit ist eine Annahme.

## 10. Quellen & Verwandtes

- **JSON-RPC 2.0 Specification**, der Umschlag im Wortlaut, einschließlich der
  reservierten Fehlercodes und der optionalen Batch- und Notification-Formen,
  die dieses Konzept ausschließt: <https://www.jsonrpc.org/specification>
  (abgerufen 2026-08-14).
- **RFC 9110 · HTTP Semantics**, Abschnitt 15, die Bedeutung der Statuscodes,
  deren Semantik F5 in das Fehlerobjekt übernimmt:
  <https://www.rfc-editor.org/rfc/rfc9110.html> (abgerufen 2026-08-14).
- **Same-Origin-Policy und CORS-Preflight**, die drei Baustellen, die F10
  vermeidet:
  <https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS> (abgerufen
  2026-08-14).
- **Vertiefung, nicht Voraussetzung:** Das Schwester-Dossier
  `single-file-static-frontend`
  (<https://github.com/schlingensiepen/proposals/tree/main/single-file-static-frontend>)
  behandelt die andere Seite derselben Anwendung: wie das Frontend ausgeliefert
  wird, das mit dieser API spricht. **Es überschneidet sich mit diesem Dossier
  an zwei Stellen** — bei der Ausnahme für Dateien (F8, F9) und bei der einen
  Origin (F10). Dort sind es Empfehlungen, hier sind es Vorgaben: Dieses Dossier
  beschreibt einen konkreten Stack, jenes ein Entwurfsmuster. Beide sind einzeln
  vollständig; wer beide liest, liest diese zwei Themen zweimal, in
  unterschiedlicher Verbindlichkeit.
