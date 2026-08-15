---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: single-file-static-frontend
language: de (führend)
question: "Wie schneide ich eine Anwendung zu, deren Auslieferung ein Kopiervorgang bleibt — und die trotzdem nichts preisgibt?"
thesis: "Das Frontend ist eine einzige statische Datei, die jeder bekommen darf; die Adresse benennt keine Ressource auf dem Server, sondern einen Zustand des Clients."
---

# Eine Datei, die jeder haben darf

**In einem Satz:** Das Frontend ist **ein** Artefakt — eine HTML-Datei mit ihren
mitgelieferten Mitteln —, das unauthentifiziert ausgeliefert wird und deshalb
keine Daten enthalten darf; alles Fachliche holt es zur Laufzeit über eine API.

**Kurzthese:** Wer sein Frontend aus einem Build-Ergebnis, einer Server-Vorlage
und einer Routing-Konfiguration zusammensetzt, deployt drei Dinge, die
zueinander passen müssen. Ein einziges statisches Artefakt macht die Auslieferung
zu einem Kopiervorgang — und weil es jeder bekommen darf, muss nichts darin
stehen, was jemand nicht haben soll. Der Preis sind unschönere Adressen, keine
serverseitige Vorbereitung und ein leerer erster Bildschirm, bis die erste
Antwort da ist.

## 1. Problem

„Ich will eine Anwendung im Browser, und ich will sie ausliefern, ohne jedes Mal
nachzudenken. Heute besteht mein Frontend aus einem Build-Ergebnis, ein paar
Vorlagen, die der Server füllt, und einer Umschreibregel, damit die Adressen
schön aussehen. Drei Dinge, die zueinander passen müssen — und wenn ich eine
Ansicht hinzufüge, muss ich daran denken, die Serverkonfiguration mitzuziehen.
Auf einem zweiten Zielsystem fange ich damit von vorn an."

Zwei Fragen stecken darin, und sie hängen zusammen:

- **Was genau wird eigentlich ausgeliefert?** Solange die Antwort „Dateien plus
  eine Konfiguration plus ein Build-Schritt" lautet, ist Auslieferung ein
  Vorgang mit Reihenfolge, Zuständen und Fehlerfällen — kein Kopieren.
- **Was steht darin, das nicht jeder sehen darf?** Sobald der Server Vorlagen
  mit Benutzerdaten füllt, ist die ausgelieferte Seite für jeden Abruf eine
  andere, und wer sie bekommt, entscheidet eine Prüfung, die jemand richtig
  gebaut haben muss.

## 2. Das Konzept

Das Frontend ist **eine** HTML-Datei. Alles, was sie braucht — Stile, Skripte,
Fremdbibliotheken —, liegt daneben, versioniert und unverändert; es gibt keinen
Build-Schritt, der aus Quellen ein Ergebnis erzeugt. Diese Datei wird
**unauthentifiziert** ausgeliefert, denn sie ist ja das, was einen Nachweis
überhaupt erst beschafft. Also darf nichts darin stehen, was nicht jeder sehen
soll. **Das Markup trägt nur Form und Hydrations-Logik** — die leere Struktur
und den Code, der sie füllt; alles Inhaltliche kommt zur Laufzeit über eine API. Im Auslieferungspfad läuft **kein Anwendungscode** — kein Server-Rendering,
keine Vorlage, die etwas einsetzt; ein Dateiserver genügt.

Damit verschiebt sich, was eine Adresse bedeutet: Sie benennt nicht mehr eine
**Ressource auf dem Server**, sondern einen **Zustand des Clients**. Der Server
hat genau einen Einstiegspunkt und liefert immer dasselbe Dokument. Der
gewünschte Zustand steht deshalb im **Fragment** — hinter dem `#` —, das der
Browser gar nicht erst beim Server nachfragt. Fremde Bibliotheken werden
mitgeliefert statt von fremden Servern geladen. Und was nicht durch eine
JSON-Schnittstelle passt — Bilder, Archive —, bekommt einen eigenen Weg.

## 3. Begriffe

- **Artefakt** — das, was ausgeliefert wird. Hier: ein Verzeichnisinhalt aus
  versionierten Dateien, ohne Zwischenschritt.
- **Build-Schritt** — ein Vorgang, der aus Quelldateien ein anderes,
  auszulieferndes Ergebnis erzeugt (Bündeln, Übersetzen, Verkleinern).
- **Datenfreies Markup** — ausgeliefertes HTML, das keine fachlichen Inhalte
  enthält, sondern nur Struktur und Beschriftungen; jeder Abruf liefert
  dieselben Bytes.
- **Hydrations-Logik** — der Code im Artefakt, der die leere Struktur zur
  Laufzeit mit Daten aus der API füllt. Er ist Teil der Form, nicht des Inhalts:
  Er beschreibt, **wohin** etwas kommt, nicht **was**.
- **Fragment** — der Teil einer Adresse hinter dem `#`. Der Browser fragt ihn
  **nicht beim Server nach**; er ist ausschließlich Sache des Clients.
- **History-API** — die Browser-Schnittstelle, mit der eine Anwendung die
  angezeigte Adresse ändert, ohne neu zu laden. Die so entstandenen Adressen
  sehen aus wie Pfade und werden beim **nächsten** Aufruf beim Server angefragt.
- **Catch-all-Regel** — eine Serverkonfiguration, die auf beliebige Pfade
  dasselbe Dokument ausliefert.
- **Einstiegspunkt** — die eine Adresse, unter der das Dokument liegt.

## 4. Festlegungen

### F1 · Ein Artefakt, kein Build-Ergebnis

**Was gilt.** Ausgeliefert wird genau das, was versioniert im Repository liegt:
eine HTML-Datei und die Dateien daneben. Es gibt keinen Schritt, der aus Quellen
etwas anderes erzeugt — kein Bündeln, kein Übersetzen, kein Verkleinern.

**Warum.** Ein Build-Schritt ist ein Zustand zwischen Quelle und Auslieferung.
Er kann veralten, auf einem anderen Rechner ein anderes Ergebnis liefern und
beim Ausliefern vergessen werden — und jedes dieser Probleme fällt erst beim
Betrieb auf, nicht beim Bauen. Ohne ihn ist die Frage „läuft dort dasselbe wie
hier?" durch einen Dateivergleich zu beantworten. Und das Zurückrollen ist
Zurückkopieren, ohne dass irgendetwas neu erzeugt werden müsste.

**Naheliegend und verworfen.** *Bündeln und Verkleinern, „das macht man doch"* —
lohnt bei großen Anwendungen mit vielen Abhängigkeiten; bei einer Anwendung, die
über eine Handvoll Dateien läuft, kauft man Ladezeit im Millisekundenbereich für
einen Werkzeugkasten, der gepflegt werden will. *Ein Build-Schritt „nur" für
die Fremdbibliotheken* — dann gibt es ihn, also braucht das Zielsystem ihn
irgendwann auch.

**Woran man merkt, dass es verletzt ist.** Jemand fragt „welcher Stand läuft
gerade?", und die Antwort verlangt, ein Werkzeug auszuführen.

### F2 · Die Datei wird unauthentifiziert ausgeliefert — und trägt deshalb keine Daten

**Was gilt.** Das Dokument ist ohne jeden Nachweis abrufbar. Genau deshalb
enthält es **ausschließlich zweierlei: Form und Hydrations-Logik** — die leere
Struktur mit ihren Beschriftungen, und den Code, der sie zur Laufzeit füllt.
**Alles Inhaltliche kommt über die API**: keine Namen, keine Listen, keine
vorbelegten Werte, keine Schlüssel, kein einziger Datensatz. Jeder Abruf liefert
dieselben Bytes.

**Die Trennlinie ist nicht „klingt fachlich", sondern „ändert sich mit den
Daten".** Eine Beschriftung wie „Lagerort" ist Form, obwohl sie aus der
Fachsprache stammt — sie steht in jedem Abruf gleich da. Ein konkreter Lagerort
ist Inhalt. Die Prüffrage im Zweifel: *Stünde hier bei einem anderen Bestand
etwas anderes?* Wenn ja, gehört es nicht ins Dokument, sondern in eine
Antwort.

**Warum.** Das ist kein Verzicht, sondern eine Bedingung. Die Anwendung ist das,
was einen Nachweis **beschafft** — man kann für ihr Laden keinen verlangen, ohne
sich im Kreis zu drehen. Da sie also jeder bekommt, ist alles darin
veröffentlicht. Der Gewinn daraus: **Das Frontend ist keine Vertrauensgrenze
mehr.** Es gibt keine Prüfung, die beim Ausliefern richtig sein muss, und keinen
Abruf, der mehr zeigt als ein anderer. Was geschützt gehört, liegt hinter der
API und wird dort geprüft — an einer Stelle statt an zweien.

**Naheliegend und verworfen.** *Den ersten Bildschirm serverseitig füllen, damit
er nicht leer ist* — genau damit wird die Auslieferung wieder zu einer
Berechtigungsfrage, und die Prüfung existiert an zwei Orten. *Die Datei hinter
eine einfache Zugangssperre stellen* — sie müsste vor dem Einladungsvorgang
greifen, also vor allem, was einen Nachweis erzeugt; das ist der Kreis von oben.
*„Nur" die Konfiguration einbetten* (Adresse der API, Anwendungsname) — sobald
etwas eingebettet wird, entsteht die Frage, was noch, und die Grenze verschiebt
sich mit jedem Wunsch. Konfiguration gehört in eine eigene, ebenso öffentliche
Datei — dann ist die Regel eindeutig statt verhandelbar.

**Woran man merkt, dass es verletzt ist.** Zwei Abrufe des Dokuments liefern
unterschiedliche Bytes.

### F3 · Im Auslieferungspfad läuft kein Anwendungscode

**Was gilt.** Das Dokument kommt von einem Dateiserver. Keine Vorlage wird
gefüllt, kein Skript läuft serverseitig, nichts wird zusammengesetzt.

**Warum.** Was nicht ausgeführt wird, kann nicht falsch ausgeführt werden. Mit
der serverseitigen Vorlage verschwindet eine ganze Fehlerklasse — eingebettete
Fremdinhalte, falsch entschärfte Werte, Vorlagen-Einbettung —, und zwar nicht,
weil jemand sie sorgfältig vermeidet, sondern weil es die Stelle nicht gibt. Das
ist der Unterschied zwischen einer Maßnahme, die gepflegt werden muss, und einer
Eigenschaft der Bauform.

Zusätzlich ist es die Voraussetzung dafür, dass die Auslieferung anderswohin
wandern kann: Ein Dateiserver, ein Objektspeicher, ein CDN — für ein Artefakt
ohne Anwendungscode sind das austauschbare Möglichkeiten.

**Naheliegend und verworfen.** *Eine kleine Vorlage, „nur für den Titel"* — sie
erzeugt genau die Stelle, um die es geht, und die nächste Anforderung setzt
etwas Fachliches hinein. *Serverseitiges Vorrendern für Suchmaschinen* —
richtig, wenn die Seite öffentlich gefunden werden soll; für eine Anwendung
hinter einem Nachweis ist es eine Auslieferung an niemanden.

**Woran man merkt, dass es verletzt ist.** Das Dokument enthält eine Stelle, an
der ein Wert eingesetzt wird.

### F4 · Die Adresse benennt einen Zustand des Clients, nicht eine Ressource des Servers

**Was gilt.** Der Server kennt genau **einen** Einstiegspunkt und liefert immer
dasselbe Dokument. Welche Ansicht die Anwendung zeigt, ist Sache des Clients;
die Adresse drückt diesen Zustand aus, benennt aber nichts, was der Server
auflösen könnte.

**Warum.** Das ist die eigentliche Verschiebung dieses Zuschnitts, und sie wird
oft übersehen: Bei einer serverseitigen Anwendung *ist* die Adresse eine
Ressource — der Server schlägt sie nach und antwortet. Hier gibt es nichts
nachzuschlagen; es gibt ein Dokument, und darin einen Zustand. Wer weiter denkt,
die Adresse sei eine Ressource, baut zwangsläufig einen Server, der so tun muss
als ob — und genau daraus entsteht die Konfiguration, die dieses Konzept
vermeiden will (F5).

Die Konsequenz ist auch, was **nicht** geht: Es gibt keine Adresse, unter der
der Server eine Teilansicht ausliefern könnte, kein serverseitiges Blättern,
keine Teilseite für einen Abruf ohne JavaScript.

**Naheliegend und verworfen.** *Die Ansichten als echte Pfade führen und den
Server sie auflösen lassen* — dann ist es keine Einzeldatei-Anwendung mehr,
sondern eine serverseitige mit Zusatz. *Für jede Ansicht eine eigene HTML-Datei*
— löst das Adressproblem und bringt alle Probleme zurück, die F1 vermeidet:
gemeinsame Teile müssen zusammengehalten werden, also braucht es einen
Build-Schritt oder Kopien.

**Woran man merkt, dass es verletzt ist.** Der Server muss wissen, welche
Ansichten die Anwendung hat.

### F5 · Der Zustand steht im Fragment, damit der Server nicht umgebogen werden muss

**Was gilt.** Der Zustand aus F4 steht **hinter dem `#`** (`…/#kiste/7`). Die
Anwendung liest ihn beim Start aus und horcht auf seine Änderung; sie benutzt
**nicht** die History-API mit pfadartigen Adressen.

**Warum.** Der Unterschied ist, was der Server zu sehen bekommt:

| | pfadartige Adresse (`/kiste/7`) | Fragment (`#kiste/7`) |
|---|---|---|
| Was der Server angefragt bekommt | genau diesen Pfad | nur den Einstiegspunkt |
| Was er dafür braucht | eine Catch-all-Regel, die **jeden** Pfad auf dasselbe Dokument legt | nichts |
| Was ausgeliefert wird | Artefakt **und** Serverkonfiguration | Artefakt |
| Aufruf im laufenden Betrieb | Wechsel per History-API ohne Neuladen | Wechsel **ohne jede Anfrage**, gleicher Kontext |

Der Kern ist die dritte Zeile. **Mit pfadartigen Adressen gehört zum Artefakt
eine Serverkonfiguration, die zu den Ansichten der Anwendung passen muss** — sie
wird auf jedem Zielsystem neu eingerichtet, sie veraltet still, wenn jemand eine
Ansicht hinzufügt, und sie ist auf verschiedenen Servern verschieden zu
schreiben. Damit ist die Auslieferung kein Kopiervorgang mehr, und der Kern
dieses Konzepts ist dahin.

Die vierte Zeile ist der praktische Zusatzgewinn: Ändert sich nur das Fragment,
**lädt der Browser gar nichts neu**. Dasselbe Dokument, derselbe
JavaScript-Kontext, derselbe Zustand im Speicher — die Anwendung zeichnet nur
neu. Vor- und Zurückblättern des Browsers funktionieren dabei wie gewohnt.

**Naheliegend und verworfen.** *History-API, weil die Adressen schöner sind* —
der einzige Grund, und er ist ein echter; er kostet aber die
Konfigurationsfreiheit. Wer eine Umgebung hat, in der eine Catch-all-Regel
ohnehin gepflegt wird, kann ihn wählen. *Beides anbieten* — dann müssen beide
Wege getestet werden, und die Anwendung trägt zwei Adressschemata für dieselbe
Sache. *Gar kein Routing, alles auf einem Bildschirm* — trägt bis zur zweiten
Ansicht, und dann kann man nichts mehr verlinken.

**Woran man merkt, dass es verletzt ist.** Nach dem Ausliefern auf ein neues
Zielsystem antwortet jede Adresse außer der Startseite mit `404`.

### F6 · Fremde Mittel werden mitgeliefert, nicht eingebunden

**Was gilt.** Fremdbibliotheken liegen als Datei im Artefakt, versioniert. Kein
Skript und kein Stil wird von einem fremden Server geladen.

**Warum.** Ein eingebundenes fremdes Skript läuft **in der eigenen Origin** und
kann alles, was eigener Code kann. Wer es ausliefert, entscheidet damit
jederzeit über den Inhalt der eigenen Anwendung — ohne dass jemand etwas
bemerkt. Das ist keine Frage des Vertrauens in eine bestimmte Adresse, sondern
eine Frage darüber, wie viele Parteien ein Mitspracherecht an der Auslieferung
haben; die richtige Antwort ist eine. Dazu kommt: Ein mitgeliefertes Mittel
funktioniert auch, wenn der fremde Server nicht erreichbar ist, und es
funktioniert in zehn Jahren noch.

**Naheliegend und verworfen.** *Einbinden mit Integritätsprüfung* — schließt den
Austausch aus, aber nicht den Ausfall, und bindet die eigene Verfügbarkeit an
eine fremde. *Einbinden, „das ist doch die verbreitete Adresse"* — Verbreitung
ist kein Schutz, sondern ein Anreiz für den, der sie übernimmt.

**Woran man merkt, dass es verletzt ist.** Die Anwendung ist im Flugzeugmodus
oder hinter einer restriktiven Firewall kaputt.

### F7 · Dateien und Binärdaten am API-Umschlag vorbei — Empfehlung

**Was gilt.** Alles, was nicht durch die Hülle des API-Protokolls passt — Bilder,
Anhänge, Archive —, sollte einen eigenen Weg bekommen: eine gesonderte Annahme
für das Hochladen und eine unmittelbare Auslieferung durch den Dateiserver für
das Herunterladen. **Wie** genau, hängt an der gewählten API-Technologie und ist
dort zu entscheiden.

**Warum.** Ein Protokoll mit einem festen Umschlag — gleich welcher Art —
transportiert Bytes nur kodiert, und jede Kodierung kostet Volumen und zwingt
die Daten vollständig in den Arbeitsspeicher. Das ist kein Detail der
Umsetzung: Es entscheidet darüber, ob der Speicherbedarf mit der **Zahl der
Anfragen** wächst oder mit der **Größe der Nutzdaten**. Wer das erst merkt, wenn
die erste große Datei kommt, muss die Schnittstelle ändern.

Dies ist eine **Empfehlung**, keine Vorgabe: Der Zuschnitt hier legt sich nicht
auf eine API-Technologie fest. Wer eine wählt, sollte diese Frage aber vor der
ersten Datei beantworten und nicht danach.

**Naheliegend und verworfen.** *Erst mal kodiert mitschicken, später ändern* —
die Änderung trifft Client und Server gleichzeitig und ist damit die teuerste
Sorte. *Dateien über die Anwendung ausliefern, um Rechte zu prüfen* — dann läuft
für jedes Vorschaubild Anwendungscode, und F3 ist für den größten Teil des
Verkehrs aufgegeben.

**Woran man merkt, dass es verletzt ist.** Der Speicherbedarf des Servers hängt
an der Größe der Nutzdaten statt an der Zahl der Anfragen.

### F8 · Auslieferung und API unter einer Origin — Empfehlung

**Was gilt.** Statisches Artefakt und API sollten unter derselben Origin liegen.
**Wie** das erreicht wird, ist offen: über Verzeichnisse unterhalb einer Basis,
über einen vorgeschalteten Vermittler, der beides zusammenführt, oder über ein
CDN mit Pfad-Weiterleitung.

**Warum.** Zwei Origins bedeuten CORS-Kopfzeilen, eine Vorabanfrage vor jedem
Aufruf mit JSON-Inhalt und Cookies, die als Drittanbieter gelten und je nach
Browsereinstellung verschwinden. Nichts davon ist unlösbar; alles davon ist
umsonst, wenn man es vermeiden kann. Ein Nachweis als Cookie funktioniert nur
unter dieser Bedingung ohne Sonderbehandlung.

Auch dies ist eine **Empfehlung**: Die Trennung kann richtig sein, wenn Frontend
und API getrennt betrieben werden oder unterschiedlich skalieren. Dann ist der
Aufwand bewusst gekauft statt versehentlich geerbt.

**Naheliegend und verworfen.** *Eine eigene Subdomain für die API, „das ist
sauberer"* — ordentlich und üblich, kostet hier drei Baustellen ohne Gegenwert.
*Das Artefakt von einem CDN, die API vom eigenen Server, ohne
Zusammenführung* — genau der Fall, in dem die drei Baustellen entstehen; mit
Pfad-Weiterleitung wäre derselbe Nutzen ohne sie zu haben.

**Woran man merkt, dass es verletzt ist.** Vor jedem Aufruf steht eine
Vorabanfrage im Zugriffslog — oder der Nachweis verschwindet in einem Browser
mit strengen Voreinstellungen.

**Tragwerk.** **F2 trägt den Sicherheitsteil, F1 den Auslieferungsteil** — und
sie treffen sich: Weil die Datei jeder bekommen darf, darf nichts drinstehen;
weil nichts drinsteht, ist das Ausliefern ein Kopiervorgang. Fällt F2, wird das
Frontend wieder eine Vertrauensgrenze und F3 verliert seinen Sinn. **F5 ruht
auf F4**, nicht umgekehrt: F4 sagt, dass die Adresse ein Client-Zustand ist, F5
zieht daraus die Konsequenz für die Schreibweise. Wer F5 aufgibt, behält F4 —
zahlt aber mit einer Serverkonfiguration und verliert damit den Kern von F1.
F6 steht unabhängig und ist einzeln übernehmbar. **F7 und F8 sind
Empfehlungen**, keine Vorgaben: Sie betreffen die Schnittstelle, auf die sich
dieser Zuschnitt nicht festlegt. **Wer nur eine Regel mitnimmt, nimmt F2.**

## 5. Sicherheitsbetrachtung

| Gefahr | Typischer Hergang | Gegenmaßnahme |
|---|---|---|
| **Das Artefakt verrät etwas** | Jemand bettet „nur" die API-Adresse ein, dann einen Anwendungsnamen, dann eine Auswahlliste der Standorte, weil sie sich „ja kaum ändert". Die Datei ist öffentlich, also ist die Liste es auch | F2 als **eindeutige** Regel: nur Form und Hydrations-Logik, alles Inhaltliche über die API. Die Prüffrage — *stünde hier bei einem anderen Bestand etwas anderes?* — entscheidet auch die Grenzfälle, die sonst verhandelt werden |
| **Serverseitig eingesetzte Werte** | Eine Vorlage füllt einen Titel aus einem Parameter; der Parameter enthält Markup | Entfällt durch F3 — es gibt keine Stelle, an der eingesetzt wird. Das ist der Unterschied zwischen einer gepflegten Maßnahme und einer Eigenschaft der Bauform |
| **Fremdes Skript in der eigenen Origin** | Ein eingebundenes Skript wird auf dem fremden Server ausgetauscht. Es läuft mit allen Rechten des eigenen Codes, liest den Zustand und kann Aufrufe mit dem Nachweis des Benutzers absetzen | F6: mitliefern statt einbinden. Eine Integritätsprüfung schließt den Austausch aus, nicht den Ausfall |
| **Alles hängt an der API** | Da das Frontend nichts prüft, ist die API die einzige Instanz, die es tut. Ein Fehler dort ist ein Fehler überall | Das ist gewollt und der Gewinn von F2: **eine** Prüfstelle statt zweier, die auseinanderlaufen. Sie muss dafür aber tatsächlich vollständig sein — kein Aufruf darf sich darauf verlassen, dass „das Frontend das ohnehin nicht anbietet" |
| **Verstecken statt schützen** | Eine Ansicht wird im Client ausgeblendet, weil der Benutzer sie nicht sehen soll — die zugehörigen Aufrufe bleiben aber offen | Nur die API entscheidet. Was im Client ausgeblendet ist, ist **nicht** geschützt; das Artefakt ist öffentlich und enthält den ausgeblendeten Zweig im Klartext |
| **Zustand in der Adresse ist sichtbar** | Das Fragment steht in der Browser-Historie und auf dem Bildschirm. Wer es für Kennungen benutzt, zeigt sie her | Das Fragment erreicht zwar keinen Server (F5), ist aber lokal sichtbar. Es taugt für Ansichten und Kennungen, die ohnehin angezeigt werden — nicht als Versteck |

**Wogegen das Konzept nicht hilft:** Es ersetzt keine Rechteprüfung — es
verschiebt sie vollständig in die API. Es schützt nicht gegen Schwachstellen im
eigenen JavaScript und nicht gegen einen kompromittierten Auslieferungsort: Wer
das Artefakt austauschen kann, tauscht die Anwendung aus.

## 6. Was das kostet

- **Der erste Bildschirm ist leer**, bis die erste Antwort da ist. Ohne
  serverseitige Vorbereitung gibt es nichts zu zeigen; ein sichtbarer
  Ladezustand ist Pflicht, kein Feinschliff.
- **Ohne JavaScript läuft nichts.** Es gibt keine Teilmenge der Anwendung, die
  ohne funktioniert.
- **Suchmaschinen sehen eine Seite.** Für eine Anwendung hinter einem Nachweis
  ist das folgenlos; für alles Öffentliche ist es ein Ausschlusskriterium.
- **Die Adressen sind unschöner.** Das `#` ist sichtbar und lässt sich nicht
  wegdiskutieren.
- **Fremde Mittel werden selbst gepflegt.** Ein mitgeliefertes Mittel bekommt
  keine Aktualisierung geschenkt; wer eine Sicherheitslücke schließen will, muss
  es bemerken und die Datei austauschen.
- **Alles zur Laufzeit heißt: mehr Rundreisen.** Was eine serverseitige
  Anwendung in einer Antwort liefert, sind hier ein Dokument und mindestens ein
  Aufruf.

## 7. Geltung und Grenzen

**Randbedingungen.** Ein Ort, der Dateien ausliefert — mehr braucht die
Auslieferung nicht. Eine API, die die Fachlogik trägt. Clients mit JavaScript.
Ein Anwendungsumfang, der ohne Bündeln auskommt: eine überschaubare Zahl von
Dateien und Abhängigkeiten.

**Kipp-Bedingungen.** Der Zuschnitt wird falsch, wenn:

- die Anwendung **öffentlich auffindbar** sein soll — Suchmaschinen und
  Vorschaubilder in Nachrichten brauchen serverseitig erzeugtes Markup;
- die Anwendung so groß wird, dass **alles auf einmal zu laden** spürbar wird —
  dann braucht es Nachladen in Teilen, und damit meist doch einen Bauschritt;
- **Zugriff ohne JavaScript** gefordert ist, aus Barrierefreiheits- oder
  Richtliniengründen;
- die **erste Darstellung messbar zählt** — Ladenverkauf, Werbeziele. Ein leerer
  erster Bildschirm ist dort ein Geschäftsproblem, kein Schönheitsfehler.

**Nicht gelöst.** Dieses Dossier sagt nichts darüber, **wie die API aussieht**,
mit der die Anwendung spricht — nur, dass es eine gibt und dass alles Fachliche
über sie läuft. Es sagt nichts über das **Zugriffsmodell**: wie ein Nachweis
entsteht, wie er transportiert und geprüft wird, wie eine Einladung funktioniert.
Es sagt nichts über den **Aufbau der Anwendung im Inneren** — Zustandshaltung,
Darstellung, Prüfung von Eingaben. Und es sagt nichts darüber, **wohin
ausgeliefert wird**; es macht nur die Auslieferung so anspruchslos, dass die
Frage offenbleiben kann.

## 8. Bewährung

> **Beispiel** (eine Inventarverwaltung; Namen und Werte erfunden). Ausgeliefert
> wird ein Verzeichnis:
>
> ```
> index.html          ← das Dokument, für jeden dieselben Bytes
> app.js              ← die Anwendung, unverändert aus dem Repository
> app.css
> mittel/qr.js        ← mitgelieferte Fremdbibliothek (F6)
> ```
>
> Ein Etikett auf einer Kiste trägt die Adresse `https://…/#7-m3xq8f`. Wer es
> antippt, löst beim Server genau eine Anfrage aus: `GET /`. Der Server liefert
> `index.html` — dieselbe Datei wie für jeden anderen Abruf, ohne zu wissen, dass
> von Kiste 7 die Rede ist. **Das Fragment hat er nie gesehen** (F5).
>
> Erst im Browser liest die Anwendung `#7-m3xq8f`, erkennt daran die gewünschte
> Ansicht und holt sich den Inhalt über die API. Tippt der Benutzer danach eine
> zweite Kiste an, ändert sich nur das Fragment: **keine Anfrage an den Server,
> kein Neuladen**, dasselbe Dokument, derselbe Kontext — die Anwendung zeichnet
> neu.
>
> Was ausgeliefert wurde, um das möglich zu machen: **die vier Dateien oben.**
> Keine Umschreibregel, kein Bauschritt, keine Vorlage. Auf ein zweites Zielsystem
> kommt man, indem man das Verzeichnis dorthin kopiert.

**Referenz-Implementierung: entfällt, weil noch keine existiert (Status:
Entwurf).** Der Zuschnitt ist vollständig entschieden und spezifiziert, die
laufende Anwendung, aus der er stammt, bildet ihn aber noch nicht ab — sie
stammt aus einer Zeit vor dieser Entscheidung und liefert mehrere HTML-Dateien
aus. Die Aussagen dieses Dossiers sind damit **durchdacht, nicht erprobt**;
insbesondere die Behauptung aus F1, dass ohne Bauschritt nichts fehlt, ist
bisher ein Argument und keine Erfahrung.

## 9. Offene Fragen

**Q1 · Wo liegt die Größengrenze von F1?** „Kein Bauschritt" trägt bei einer
Handvoll Dateien. Ab wann kippt es — bei zwanzig Modulen, bei der ersten
Abhängigkeit, die nur als Paket vorliegt? Gesucht ist ein Kriterium, das man
**vorher** anwenden kann, nicht die Erkenntnis nach dem ersten langsamen Laden.

**Q2 · Wie hält man mitgelieferte Fremdmittel aktuell?** F6 nimmt die Pflege in
Kauf, sagt aber nicht, wie man von einer Sicherheitslücke erfährt, wenn kein
Paketmanager mitliest. Ein Vermerk der Fassung im Artefakt ist das Mindeste —
reicht das?

**Q3 · Ist der leere erste Bildschirm wirklich folgenlos?** Bei einer Anwendung
hinter einem Nachweis wird gern behauptet, es zähle nur die Bedienbarkeit. Ab
welcher Wartezeit stimmt das nicht mehr, und ist ein aus dem Artefakt
gezeichnetes Gerüst (ohne Daten) noch mit F2 vereinbar?

**Q4 · Trägt F5 auch bei tiefen Zuständen?** Ein Fragment ist eine flache
Zeichenkette. Für „welche Ansicht, welches Objekt" reicht das; für Filter,
Sortierung, Blätterstand und geöffnete Bereiche wird es schnell zu einem eigenen
kleinen Format. Ab wann ist das ein Fehler und kein Zustand mehr?

## 10. Quellen & Verwandtes

- **Fragment-Bezeichner in Adressen**, RFC 3986 Abschnitt 3.5 — die Festlegung,
  dass der Fragment-Teil **nicht** an den Server übertragen wird, auf der F5
  beruht: <https://www.rfc-editor.org/rfc/rfc3986#section-3.5> (abgerufen
  2026-08-15).
- **History-API**, die Gegenoption aus F5, mitsamt dem Hinweis, dass die so
  erzeugten Adressen beim nächsten Aufruf beim Server ankommen:
  <https://developer.mozilla.org/en-US/docs/Web/API/History_API> (abgerufen
  2026-08-15).
- **Same-Origin-Policy und CORS-Vorabanfrage**, die Baustellen, die F8
  vermeidet: <https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS>
  (abgerufen 2026-08-15).
- **Vertiefung, nicht Voraussetzung — die Gegenseite:** Das Schwester-Dossier
  `single-endpoint-json-rpc`
  (<https://github.com/schlingensiepen/proposals/tree/main/single-endpoint-json-rpc>)
  beschreibt eine API, die zu diesem Zuschnitt passt. **Es überschneidet sich
  mit diesem Dossier an zwei Stellen** — der Ausnahme für Dateien (hier F7) und
  der einen Origin (hier F8). Dort sind es **Vorgaben**, weil ein konkreter
  Stack beschrieben wird; hier sind es **Empfehlungen**, weil dieser Zuschnitt
  sich auf keine API-Technologie festlegt. Beide sind einzeln vollständig.
- **Vertiefung, nicht Voraussetzung — der Zugang:** Wie ein Nachweis entsteht,
  wie eine Einladung funktioniert und warum ein Einladungslink sein Geheimnis im
  Fragment tragen kann, ohne in ein Zugriffslog zu geraten, ist Gegenstand eines
  eigenen Dossiers zum Zugriffsmodell. Für diesen Zuschnitt genügt: Ein solcher
  Link **funktioniert weiterhin**, weil F5 das Fragment ohnehin auswertet.
