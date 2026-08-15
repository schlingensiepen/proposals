---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: automated-deployment-that-keeps-its-secrets
language: de (führend)
question: "Wie liefere ich ein System aus mehreren Teilen automatisiert aus, richte dabei die Zugangskontrolle mit ein — und verrate weder die Zugänge zu den Trägersystemen noch die Geheimnisse, die dabei entstehen?"
thesis: "Je Komponente ein versioniertes Skript, je Instanz ein Verzeichnis außerhalb der Versionierung; das Skript besitzt die Geheimnisse, erzeugt die Zugänge in einem befristeten Fenster — und daraus, nicht aus einer Konvention, folgt die Reihenfolge der Teil-Deployments."
---

# Ein Deployment, das seine Geheimnisse behält

**In einem Satz:** Ausgeliefert wird von einem versionierten Skript je
Komponente, gesteuert von einem gitignorierten Verzeichnis je Instanz; das
Skript erzeugt und setzt die Geheimnisse des Systems selbst — und weil ein Teil
seine Zugänge nur bekommt, solange ein anderer ein Fenster offen hält, ergibt
sich die Reihenfolge aus dem Geheimnisfluss statt aus einer Absprache.

**Kurzthese:** „Automatisiertes Deployment" wird meist als Übertragungsproblem
gelöst — Dateien hinschieben, fertig. Sobald ein System aus mehreren Teilen
besteht und Zugangskontrolle braucht, ist es aber ein **Geheimnisproblem**: Es
gibt Zugänge zu den fremden Rechnern, auf die ausgeliefert wird, und Zugänge,
die beim Ausliefern erst **entstehen**. Wer beide sauber trennt und keinen von
beiden in die Versionierung lässt, bekommt die Reihenfolge der Teile geschenkt.
Der Preis ist ein zweites Verzeichnis je Instanz, das niemand sichert außer dem
Betreiber selbst.

## 1. Problem

„Mein System besteht aus drei Teilen — einer Anwendung auf einem gemieteten
Webspace, einem Hintergrundprozess auf einem Rechner bei mir, einer Sicherung
auf einem dritten. Jeder wird anders ausgeliefert. Jeder braucht Zugangsdaten,
um dort hinzukommen. Und der Hintergrundprozess braucht einen Schlüssel für die
Anwendung, den es vorher gar nicht gibt. Wo lege ich das alles hin, ohne dass
es im Repository landet — und in welcher Reihenfolge mache ich das?"

Darin stecken **zwei Arten von Geheimnissen**, die regelmäßig vermengt werden:

| | Beispiel | Eigenschaft |
|---|---|---|
| **Zugang zum Trägersystem** | SSH-Schlüssel, Passwort für die Dateiübertragung | existiert **vorher**, gehört dem Betreiber, wird nur **benutzt** |
| **Geheimnisse des Systems** | Signaturschlüssel, Deploy-Schlüssel, Maschinenzugänge | entsteht **beim Ausliefern**, gehört dem System, wird **erzeugt und eingesetzt** |

Die erste Art darf nie ins Repository. Die zweite darf es auch nicht — und aus
ihr folgt zusätzlich, in welcher **Reihenfolge** die Teile ausgeliefert werden
müssen. Das ist der Zusammenhang, den man nur sieht, wenn man beide zusammen
betrachtet.

## 2. Das Konzept

Für jede **Komponente** gibt es ein Skript, und für jede **Instanz** ein
Verzeichnis. Das Skript ist versioniert und kennt kein Ziel; das Verzeichnis
liegt außerhalb der Versionierung und kennt genau eines — es trägt Adresse,
Zugangsdaten und alle Einstellungen dieser Instanz. Aufgerufen wird das Skript
mit dem Verzeichnis als einzigem Parameter.

Was übertragen wird, ist **eine Liste aus der Versionierung**, nicht der Inhalt
eines Verzeichnisses. Laufzeitdaten sind im Skriptkern fest gesperrt, nicht
durch eine Konfigurationszeile ausgenommen. Die produktive Konfiguration legt
das Skript an, wenn sie fehlt, und überschreibt sie **nie** — außer auf
ausdrückliche Anweisung, die benennt, was dabei zerstört wird.

Die Geheimnisse des Systems erzeugt das Skript selbst und legt sie neben die
Instanzkonfiguration. Zugänge für andere Komponenten entstehen in einem
**befristeten Fenster**, das die Auslieferung öffnet und das von selbst zufällt.
Daraus folgt die Reihenfolge: Wer sein Geheimnis nur im offenen Fenster
bekommen kann, wird nach dem ausgeliefert, der es öffnet.

Über allem steht ein Einstieg, der Ziele ausführt, anlegt und auflistet. Und zu
jeder Auslieferungsart gehört eine Anleitung, wie dasselbe **von Hand** geht.

## 3. Begriffe

- **Komponente** — ein Teil des Systems, der eigenständig ausgeliefert wird:
  Anwendung, Hintergrundprozess, Sicherungsdienst.
- **Instanz** — ein konkretes Ziel einer Komponente: dieser Webspace, dieser
  Rechner. Eine Komponente kann viele Instanzen haben.
- **Instanzverzeichnis** — der gitignorierte Ordner mit allem, was diese eine
  Instanz ausmacht: Adresse, Zugangsdaten, Einstellungen, erzeugte Geheimnisse.
- **Trägersystem** — der fremde Rechner, auf den ausgeliefert wird.
- **Zugangsgeheimnis** — womit das Skript auf das Trägersystem kommt.
- **Systemgeheimnis** — was beim Ausliefern entsteht und im System wirkt.
- **Fenster** — ein befristeter Zustand, in dem Vorgänge erlaubt sind, die im
  laufenden Betrieb verboten sind.

## 4. Festlegungen

### F1 · Je Komponente ein Skript, je Instanz ein Verzeichnis

**Was gilt.** Das Auslieferungsskript ist **versioniert** und kennt kein Ziel.
Das Instanzverzeichnis liegt **außerhalb der Versionierung** und kennt genau
eines. Der einzige Aufrufparameter ist das Verzeichnis. Beide gehören nie in
dasselbe Repository.

**Warum.** Die Trennung verläuft entlang der Frage, wer etwas ändern darf: Am
Skript ändert, wer die Komponente entwickelt; am Verzeichnis, wer die Instanz
betreibt. Vermischt man beides, muss jeder Betreiber das Repository anfassen,
um seine Adresse einzutragen — und dann steht sie darin. Der Zuschnitt „je
Komponente" statt „ein Skript für alles" folgt derselben Logik: Ein Webspace
wird anders ausgeliefert als ein Rechner, den man selbst betreibt, und ein
gemeinsames Skript mit Fallunterscheidungen wird bei der dritten Komponente
unlesbar.

**Naheliegend und verworfen.** *Ein Skript mit Zielen als Parameter* — spart
Dateien und kostet Trennschärfe; jede Besonderheit einer Komponente wird zu
einem `if`. *Konfiguration im Repository, Geheimnisse als Umgebungsvariablen* —
funktioniert für Zugänge, nicht für Erzeugtes: Ein Signaturschlüssel, der beim
ersten Lauf entsteht, muss irgendwo **bleiben**. *Ein Verzeichnis für alle
Instanzen* — dann liegen die Zugangsdaten fremder Rechner beieinander, und ein
Fehlgriff trifft alle.

**Woran man merkt, dass es verletzt ist.** Im Repository steht eine Adresse,
ein Benutzername oder ein Pfad, der nur für ein einziges Ziel gilt.

### F2 · Das Skript besitzt die Geheimnisse des Systems

**Was gilt.** Was beim Ausliefern entstehen muss — Signaturschlüssel,
Deploy-Schlüssel, Maschinenzugänge —, **erzeugt das Skript**, schreibt es an
seinen Platz auf dem Trägersystem und sichert es zusätzlich neben der
Instanzkonfiguration. Kein Mensch denkt sich eines aus, keines steht je in der
Versionierung.

**Warum.** Ein von Hand ausgedachtes Geheimnis ist das schwächere, und ein von
Hand eingetragenes wandert durch Zwischenablagen, Chatfenster und Notizzettel.
Vor allem aber: Wer die Erzeugung dem Menschen überlässt, hat keinen Zeitpunkt,
an dem sie **sicher** geschieht — sie geschieht dann, wenn jemand daran denkt.

**Die zweite Sicherung ist kein Luxus.** Manche Systemgeheimnisse sind
unersetzlich: Ein Signaturschlüssel, mit dem dauerhafte Adressen unterschrieben
wurden, macht bei Verlust jede dieser Adressen wertlos — und eine Datensicherung
umfasst ihn üblicherweise nicht, denn sie sichert Daten, nicht Konfiguration.
Deshalb liegt eine Kopie neben der Instanzkonfiguration, mit engen Rechten, und
das Skript nennt sie beim ersten Lauf im Terminal.

**Naheliegend und verworfen.** *Geheimnisse beim ersten Start der Anwendung
erzeugen lassen* — dann entstehen sie beim ersten zufälligen Besucher, und
niemand weiß, wann. *Sie in die Instanzkonfiguration eintragen lassen* — der
Weg, auf dem sie in Zwischenablagen landen. *Auf die zweite Sicherung
verzichten* — trägt genau so lange, bis das Trägersystem verlorengeht.

**Woran man merkt, dass es verletzt ist.** Jemand fragt, welchen Wert er wo
eintragen soll.

### F3 · Ausgeliefert wird eine Liste aus der Versionierung

**Was gilt.** Der Paketinhalt ist die Liste der **versionierten** Dateien einer
Komponente — nicht der Inhalt ihres Verzeichnisses auf dem Rechner des
Ausliefernden.

**Warum.** Im Arbeitsverzeichnis liegen Testdaten, eigene Konfigurationen,
Protokolle und halbfertige Versuche. Wer das Verzeichnis überträgt, überträgt
sie mit — und merkt es nicht, weil auf dem Zielsystem nichts kaputtgeht.
„Was versioniert ist" ist die einzige Grenze, die sich ohne Pflegeaufwand
prüfen lässt: Sie steht ohnehin schon fest.

**Was daraus folgt und leicht übersehen wird:** Schutzdateien, die in einem
sonst ignorierten Verzeichnis liegen — die Regel, die das Auflisten eines
Ordners verhindert, die Beispielkonfiguration —, müssen **ausdrücklich
mitversioniert** werden. Sonst kommen sie nie auf den Server, und das
Verzeichnis steht offen, während die Dokumentation das Gegenteil behauptet.

**Naheliegend und verworfen.** *Das Verzeichnis übertragen und Ausnahmen
pflegen* — die Ausnahmeliste veraltet, sobald jemand eine neue Testdatei
anlegt. *Ein Bauschritt, der ein Paket zusammenstellt* — eine weitere Stufe mit
eigenem Zustand; wenn die Liste ohnehin existiert, ist sie das Paket.

**Woran man merkt, dass es verletzt ist.** Auf dem Zielsystem liegt eine Datei,
die niemand dorthin gelegt hat.

### F4 · Laufzeitdaten sind fest gesperrt, nicht per Konfiguration ausgenommen

**Was gilt.** Was auf dem Trägersystem zur Laufzeit entsteht — Bestand,
Anhänge, Sicherungen, die produktive Konfiguration — wird vom Skript **nie**
angefasst. Diese Sperre steht im **Kern** des Skripts, nicht in einer
Konfigurationsdatei.

**Warum.** Eine Sperre, die konfigurierbar ist, ist eine Sperre, die jemand
konfiguriert. Und der Fehler, den sie verhindert, ist nicht reparabel: Ein
überschriebener Bestand ist weg. Die Instanzkonfiguration darf über **Adressen
und Zugänge** entscheiden, nicht darüber, was gelöscht werden darf.

**Naheliegend und verworfen.** *Ausschlussliste in der Instanzkonfiguration* —
bequem und der wahrscheinlichste Weg zum Datenverlust; sie wird kopiert,
gekürzt, angepasst. *Vor dem Übertragen alles löschen, „damit es sauber ist"* —
das ist ein Abgleich, kein Deployment; entfernte Dateien bleiben stehen, und
das ist billiger als die Alternative.

**Woran man merkt, dass es verletzt ist.** In der Instanzkonfiguration steht
ein Pfad, der von einer Löschung ausgenommen werden soll.

### F5 · Die produktive Konfiguration wird angelegt, nie überschrieben — außer auf Anweisung

**Was gilt.** Fehlt die produktive Konfiguration auf dem Trägersystem, legt das
Skript sie an und füllt sie. Ist sie vorhanden, bleibt sie **unangetastet**,
auch feldweise. **Ausnahme:** ein ausdrücklicher Schalter (`--force`), der nicht
voreingestellt sein darf und der vor dem Ausführen **benennt, was er zerstört**.

**Warum.** Beim ersten Lauf gibt es niemanden, der die Zufallswerte erzeugt —
also muss das Skript sie anlegen können. Bei jedem weiteren Lauf enthält die
Datei Werte, die es nicht kennt und nicht rekonstruieren kann; ein
Überschreiben wäre ein stiller Totalverlust.

**Warum es den Schalter trotzdem geben muss:** Ohne ihn gibt es Lagen, aus
denen man nicht herauskommt — ein gewechselter Deploy-Schlüssel, eine geänderte
Adresse, eine beschädigte Datei. Die Antwort „dann von Hand" ist keine, denn
genau dort passieren die Fehler, die das Skript vermeiden soll. **Ein
gefährlicher Vorgang gehört ins Werkzeug, nicht aus ihm heraus** — aber
ausdrücklich, benannt und nie als Voreinstellung.

**Was der Schalter sagen muss, bevor er handelt:** welche Werte er ersetzt und
was das bedeutet. Bei einem Signaturschlüssel etwa: dass jede bisher
unterschriebene Adresse danach ungültig ist. Ein `--force`, das nur „überschreibe
Konfiguration?" fragt, verlagert die Entscheidung auf jemanden, der die Folge
nicht kennt.

**Naheliegend und verworfen.** *Immer überschreiben und die Werte vorher
auslesen* — setzt voraus, dass man sie auslesen kann; bei einem beschädigten
System gerade nicht. *Nie überschreiben, notfalls von Hand* — schiebt den
gefährlichsten Vorgang dorthin, wo es keine Prüfung und keine Warnung gibt.
*Feldweise zusammenführen* — klingt schonend und ist der schlimmste Fall: Man
weiß hinterher nicht, welcher Wert von wo stammt.

**Woran man merkt, dass es verletzt ist.** Ein zweiter Lauf des Skripts
verändert eine Datei, die beim ersten entstanden ist — oder es gibt keinen Weg,
einen kompromittierten Schlüssel auszutauschen.

### F6 · Zugänge für andere Komponenten entstehen in einem befristeten Fenster

**Was gilt.** Zugänge, die eine Komponente **für eine andere** braucht, werden
nicht im laufenden Betrieb erzeugt, sondern in einem Fenster, das die
Auslieferung öffnet und das nach einer festen Frist von selbst zufällt. Ihr
Klartext erscheint **genau einmal**.

**Warum.** Solche Zugänge sind dauerhaft und mächtig — ein Sicherungszugang gibt
den gesamten Bestand heraus. Könnte man sie jederzeit aus der laufenden
Anwendung erzeugen, genügte ein übernommener Verwaltungszugang, um sich einen
zu verschaffen, der die Sperrung des Verwalters überlebt. Das Fenster verlegt
diesen Vorgang an die einzige Stelle, an der ohnehin jemand mit vollem Zugriff
handelt: die Auslieferung.

**Die Vertrauensgrenze verläuft dabei nicht am Fenster.** Wer es öffnen kann,
kann auf das Trägersystem schreiben — er könnte die Anwendung austauschen. Das
Fenster gibt ihm nichts, was er nicht schon hätte; es macht es nur **befristet
bequem erreichbar**. Praktisch genügt dafür eine kleine Datei neben der
Konfiguration mit einem Geheimnis und einem Ablaufzeitpunkt, geprüft durch einen
Vergleich mit der Uhr — kein Zeitgeber, kein Hintergrundprozess.

**Und die Gegenrichtung ist ausdrücklich anders:** Ein solcher Zugang lässt sich
**jederzeit** im laufenden Betrieb stilllegen, ohne Fenster und ohne
Auslieferung. Ein Vorgang, der Rechte wegnimmt, darf nie schwerer erreichbar
sein als der, der sie gibt.

**Naheliegend und verworfen.** *Erzeugung im Verwaltungsbereich lassen* —
bequem, und sie macht einen erbeuteten Verwaltungszugang zum Generalschlüssel
mit unbegrenzter Haltbarkeit. *Eine Datei, die man nach der Einrichtung löscht* —
verlässt sich darauf, dass jemand daran denkt; vergessen wird sie in genau den
Fällen, in denen es darauf ankäme. *Auch das Stilllegen ans Fenster binden* —
klingt konsequent und lässt eine übernommene Komponente bis zur nächsten
Auslieferung weiterlaufen.

**Woran man merkt, dass es verletzt ist.** Ein Maschinenzugang lässt sich aus
der laufenden Anwendung heraus erzeugen — oder nur bei offenem Fenster
stilllegen.

### F7 · Die Reihenfolge folgt dem Geheimnisfluss, nicht einer Konvention

**Was gilt.** In welcher Reihenfolge die Komponenten ausgeliefert werden, ist
**keine Absprache**, sondern eine technische Abhängigkeit: Wer sein Geheimnis
nur bei offenem Fenster bekommen kann, wird nach dem ausgeliefert, der das
Fenster öffnet. Das Werkzeug setzt die Reihenfolge durch, statt sie zu
dokumentieren.

**Warum.** Eine dokumentierte Reihenfolge ist eine, an die man sich erinnern
muss — und der Fehlerfall ist unauffällig: Die falsch herum ausgelieferte
Komponente läuft, sie hat nur keinen Zugang und meldet das erst beim nächsten
zeitgesteuerten Lauf, womöglich nachts. **Das ist der eigentliche Ertrag dieses
Konzepts:** Nimmt man F2 und F6 zusammen, ergibt sich die Reihenfolge von
selbst; man muss sie nicht erfinden, nur ablesen.

**Nur beim ersten Mal je Instanz.** Danach liegt der Klartext lokal, und die
Reihenfolge ist gleichgültig — die Abhängigkeit besteht zwischen *Erzeugung* und
*Empfang*, nicht zwischen den Komponenten als solchen.

**Naheliegend und verworfen.** *Die Reihenfolge in die Anleitung schreiben* —
sie wird gelesen, wenn etwas schiefgegangen ist, nicht vorher. *Jede Komponente
wartet, bis ihr Zugang da ist* — verlagert eine Reihenfolge in eine
Warteschleife und erzeugt einen Zustand, den jemand beenden muss.

**Woran man merkt, dass es verletzt ist.** Es gibt einen Satz der Form „erst A
ausliefern, dann B" in einer Anleitung, aber nichts, das ihn erzwingt.

### F8 · Ein Einstieg, der ausführt, anlegt und auflistet

**Was gilt.** Über den Einzelskripten steht ein Einstieg mit drei Betriebsarten:
Ziele **ausführen** (in der von F7 erzwungenen Reihenfolge), ein Ziel **anlegen**
(Verzeichnis mit engen Rechten, Konfigurationsvorlage hinein, nichts
übertragen), und **auflisten**, was vorhanden ist.

**Warum.** Die Instanzverzeichnisse sind gitignoriert — sie stehen weder im
Repository noch in der Dokumentation. Wer nach Monaten zurückkommt, weiß weder,
wie seine Ziele heißen, noch in welcher Reihenfolge sie drankommen. Beides kann
ein Werkzeug beantworten, und dann muss es niemand erinnern.

**Die Vorlage gehört zum Skript, nicht zum Einstieg.** Neben jedem
Auslieferungsskript liegt versioniert seine Musterkonfiguration; der Einstieg
kopiert sie und kennt ihren Inhalt nicht. Sonst stünde die Schlüsselliste an
zwei Stellen und liefe auseinander, sobald ein Skript eine neue braucht.

**Naheliegend und verworfen.** *Nur die Einzelskripte, Reihenfolge und Namen in
der Anleitung* — verlagert drei Fragen in ein Dokument, das niemand offen hat,
wenn er ausliefert. *Der Einstieg fragt die Werte beim Anlegen interaktiv ab* —
ein zweiter Konfigurationsweg mit eigener Prüfung und der Frage, was bei einem
Abbruch zurückbleibt; eine kommentierte Vorlage leistet dasselbe und ist in
einem halben Jahr noch lesbar.

**Woran man merkt, dass es verletzt ist.** Jemand muss in der Dokumentation
nachsehen, wie seine eigenen Ziele heißen.

### F9 · Jedes Skript prüft von außen nach, was es angerichtet hat

**Was gilt.** Nach der Übertragung prüft das Skript das Ergebnis — und zwar die
Fragen, die **nur von außen** zu beantworten sind: Ist ein Verzeichnis öffentlich
lesbar, das es nicht sein darf? Wird eine Datei ausgeliefert, die ausgeführt
werden müsste, oder umgekehrt? Das Ergebnis steht im Terminal, und ein Befund
erzeugt einen eigenen Rückgabewert.

**Warum.** Ein Prüfprogramm **auf** dem Trägersystem kann „was sieht ein
Fremder?" nicht beantworten: Es müsste sich selbst über das Netz aufrufen und
umginge dabei womöglich genau den Zwischenserver, um den es geht. Nur wer außen
steht, sieht, was außen sichtbar ist — und das Auslieferungsskript steht außen.

**Und es ist der einzige Zeitpunkt, an dem jemand hinsieht.** Danach läuft das
System, und ein offenes Verzeichnis fällt erst auf, wenn es zu spät ist.

**Naheliegend und verworfen.** *Der Anwendung eine Selbstprüfung geben* — sie
kann alles prüfen außer dem, worauf es ankommt. *Die Prüfung als eigenen
Vorgang, den man gelegentlich anstößt* — „gelegentlich" ist nie.

**Woran man merkt, dass es verletzt ist.** Eine Fehlkonfiguration des
Trägersystems fällt bei einem Sicherheitsvorfall auf und nicht bei der
Auslieferung.

### F10 · Zu jeder Auslieferungsart gehört eine Anleitung für den Handbetrieb

**Was gilt.** Für jede Komponente und jeden Übertragungsweg gibt es zusätzlich
eine Anleitung, die **dasselbe von Hand** beschreibt — Erstinstallation und
Wanderung des Bestands eingeschlossen. Sie ist Teil der Auslieferung, nicht
Zugabe.

**Warum.** Das Skript setzt Dinge voraus, die nicht überall gelten: eine
Laufzeitumgebung, einen Übertragungsweg, eine Netzverbindung zum richtigen
Zeitpunkt. Fällt eine davon aus, steht der Betreiber vor einem System, das er
nicht ausliefern kann — und improvisiert. **Genau dann entstehen die Fehler,
die das Skript verhindern soll**, nur ohne dessen Sperren und ohne dessen
Prüfung.

Der Zweiklang lautet: **nimm das Skript, und wenn das nicht geht, folge der
Anleitung.** Beides muss vorhanden sein, sonst ist es kein Zweiklang, sondern
eine Hoffnung.

**Ein Nebeneffekt, der die Anleitung rechtfertigt, auch wenn sie nie gebraucht
wird:** Wer sie schreibt, muss benennen, was das Skript tut. Ein Vorgang, der
sich nicht in Handgriffen beschreiben lässt, ist meist auch im Skript nicht
sauber getrennt.

**Naheliegend und verworfen.** *Auf die Anleitung verzichten, „das Skript ist ja
dokumentiert"* — Quelltext ist keine Anleitung, und im Störfall liest ihn
niemand. *Eine gemeinsame Anleitung für alle Wege* — die Wege unterscheiden
sich genau in den Handgriffen, um die es geht.

**Woran man merkt, dass es verletzt ist.** Auf die Frage „wie mache ich das ohne
das Skript?" gibt es keine Antwort außer „ins Skript schauen".

**Tragwerk.** **F1 trägt alles** — ohne die Trennung von Skript und Instanz gibt
es keinen Ort, an dem Geheimnisse liegen dürften. **F2 und F6 zusammen erzeugen
F7:** Erst wenn das Skript die Systemgeheimnisse besitzt *und* sie nur in einem
Fenster entstehen, folgt die Reihenfolge aus der Sache statt aus einer
Absprache; fällt eines von beiden, wird F7 wieder zur Konvention. **F3, F4 und
F5 schützen den Bestand** und sind einzeln übernehmbar — F5 ist dabei der
gefährlichste, weil sein `--force` bei falscher Ausführung genau den Schaden
anrichtet, den F4 verhindert. F8 ist Bequemlichkeit mit einem sicherheitsnahen
Nebeneffekt: Ein Werkzeug, das die Reihenfolge kennt, wird nicht falsch bedient.
F9 und F10 stehen unabhängig. **Wer nur eine Regel mitnimmt, nimmt F1.**

## 5. Sicherheitsbetrachtung

| Gefahr | Typischer Hergang | Gegenmaßnahme |
|---|---|---|
| **Zugangsdaten im Repository** | Jemand trägt Adresse und Passwort „nur zum Testen" in eine versionierte Datei ein. Sie bleibt in der Historie, auch nach dem Entfernen | F1: Zugänge liegen ausschließlich im gitignorierten Instanzverzeichnis. Die Musterregel schließt **alle** Instanzverzeichnisse ein, nicht einzelne Namen — sonst wird das zweite Ziel eingecheckt |
| **Systemgeheimnis im Paket** | Die produktive Konfiguration liegt im Arbeitsverzeichnis und wird beim Übertragen des Ordners mitgenommen | F3: übertragen wird die Liste der versionierten Dateien. Wer stattdessen den Ordner überträgt, verteilt seine eigenen Geheimnisse |
| **Bestand überschrieben** | Eine Ausschlussliste wird kopiert und gekürzt; beim nächsten Lauf fehlt ein Pfad | F4: Sperre im Skriptkern, nicht in der Konfiguration. Der Fehler ist nicht reparabel |
| **`--force` ohne Verständnis** | Ein Lauf scheitert, jemand hängt `--force` an, und der Signaturschlüssel wird neu erzeugt — alle bisher unterschriebenen Adressen sind wertlos | F5: Der Schalter **benennt vor dem Handeln, was er zerstört**. Eine Rückfrage „überschreiben?" verlagert die Entscheidung auf jemanden, der die Folge nicht kennt |
| **Dauerhafter Zugang aus einem erbeuteten Verwaltungszugang** | Wer ein Verwaltergerät übernimmt, stellt sich einen Maschinenzugang aus, der die Sperrung überlebt | F6: solche Zugänge entstehen nur im Fenster — und lassen sich trotzdem jederzeit stilllegen |
| **Fenster bleibt offen** | Es wird von Hand geöffnet und niemand schließt es | F6: feste Frist, Vergleich mit der Uhr, kein manuelles Schließen vorgesehen |
| **Offenes Verzeichnis auf dem Trägersystem** | Eine Schutzdatei liegt in einem ignorierten Ordner und wurde nie mitversioniert; auf dem Server fehlt sie | F3 (Rückausnahmen ausdrücklich versionieren) **und** F9 (von außen nachprüfen). Keiner der beiden allein genügt |
| **Instanzverzeichnis verloren** | Neuer Rechner, gelöschter Ordner — die zweite Sicherung des Signaturschlüssels ist weg | F2: die Kopie neben der Instanzkonfiguration, mit engen Rechten. Sie ist der einzige Schutz; eine Datensicherung des Systems umfasst sie nicht |

**Wogegen das Konzept nicht hilft:** Es schützt nicht gegen ein kompromittiertes
Trägersystem und nicht gegen einen kompromittierten Rechner des Ausliefernden —
wer dort schreiben kann, kontrolliert beides. Es ersetzt keine Rechteverwaltung
im System selbst; es richtet sie nur ein.

## 6. Was das kostet

- **Ein zweites Verzeichnis je Instanz, das niemand außer dem Betreiber
  sichert.** Geht es verloren, sind die darin gesicherten Systemgeheimnisse weg —
  und mit einem Signaturschlüssel womöglich alles, was er unterschrieben hat.
- **Je Komponente und Übertragungsweg ein eigenes Skript**, plus je eine
  Anleitung für den Handbetrieb (F10). Das ist mehr Material, als „ein
  Deploy-Skript" vermuten lässt.
- **Die erste Auslieferung ist ein Sonderfall.** Sie erzeugt Geheimnisse und
  legt Konfigurationen an; alle weiteren nicht. Zwei Verhaltensweisen in einem
  Werkzeug, und die seltenere ist die gefährlichere.
- **`--force` existiert.** Damit gibt es einen Weg, alles zu zerstören, und er
  ist dokumentiert. Das ist die bewusste Entscheidung aus F5, aber es bleibt ein
  Weg.
- **Die Reihenfolge gilt nur beim ersten Mal je Instanz** — danach ist sie
  gleichgültig. Wer sie als eherne Regel merkt, wundert sich später.

## 7. Geltung und Grenzen

**Randbedingungen.** Mehrere Komponenten, die getrennt ausgeliefert werden. Ein
Betreiber, der die Instanzverzeichnisse besitzt und verwahrt. Trägersysteme, auf
die man Dateien übertragen kann. Systemgeheimnisse, die beim Ausliefern
entstehen dürfen.

**Kipp-Bedingungen.** Der Zuschnitt wird falsch, wenn:

- eine **Geheimnisverwaltung** vorhanden ist, die Erzeugung, Ablage und Rotation
  bereits leistet — dann ist F2 überflüssig und die Instanzverzeichnisse
  schrumpfen auf Adressen;
- die Auslieferung **unbeaufsichtigt** aus einer Pipeline läuft — dann gibt es
  keinen Menschen, der ein Fenster nutzt oder eine `--force`-Warnung liest, und
  F5 bis F7 brauchen eine andere Antwort;
- **unveränderliche Trägersysteme** verwendet werden, die bei jeder Auslieferung
  neu entstehen — dann gibt es keine bestehende Konfiguration, die man schützen
  müsste, und F4 und F5 verlieren ihren Gegenstand;
- eine Komponente **viele gleichartige Instanzen** hat — je Instanz ein
  handgepflegtes Verzeichnis trägt bei einer Handvoll, nicht bei hundert.

**Nicht gelöst.** Wie die Anwendung ihre Rechte verwaltet — dieses Konzept
richtet die Zugangskontrolle ein, es entwirft sie nicht. Wie ein Rückbau
aussieht (Zurückrollen einer misslungenen Auslieferung). Wie Systemgeheimnisse
**turnusmäßig** gewechselt werden; F5 schafft nur die Möglichkeit. Und wie
mehrere Ausliefernde sich abstimmen — hier wird durchgehend einer angenommen.

## 8. Bewährung

> **Beispiel** (ein privates Inventarsystem; Namen und Werte erfunden). Drei
> Komponenten: die Anwendung auf einem gemieteten Webspace, ein
> Auswertungsprozess auf einem Rechner mit Grafikkarte, ein Sicherungsprozess
> auf einem kleinen Server.
>
> **Was im Repository liegt:** `deploy/deploy-sftp.py`, `deploy/worker-scp.py`,
> je eine Musterkonfiguration daneben, und `deploy/deploy.py` als Einstieg.
> Kein Zielname, keine Adresse.
>
> **Was daneben liegt**, gitignoriert: `deploy-ionos/` mit Adresse,
> Zugangsdaten und — nach dem ersten Lauf — der Kopie des Signaturschlüssels;
> `worker-bastelrechner/` und `worker-nas/` mit je Adresse, SSH-Schlüssel und
> der Angabe, welche Aufgabe dieser Rechner hat.
>
> **Der erste Lauf.** `deploy.py deploy-ionos worker-bastelrechner worker-nas`
> liefert **zuerst** den Webspace aus — nicht, weil es so in der Anleitung
> steht, sondern weil die beiden anderen ihre Schlüssel nur bekommen, solange
> dessen Fenster offen ist (F7). Dabei entstehen Signaturschlüssel und
> Deploy-Schlüssel, werden auf den Server geschrieben und neben
> `deploy-ionos/` gesichert; das Skript nennt sie im Terminal (F2). Danach
> prüft es von außen, ob das Datenverzeichnis lesbar ist und ob im
> Anhangsverzeichnis ein Skript ausgeführt würde (F9).
>
> Anschließend holen sich die beiden Worker-Ziele im offenen Fenster **je einen
> eigenen Schlüssel** — der eine für *auswerten*, der andere für *sichern* —
> und schreiben ihn in ihre Instanzkonfiguration (F6).
>
> **Der zweite Lauf**, eine Woche später, überträgt nur Dateien: Die
> Konfiguration auf dem Server bleibt unangetastet (F5), der Bestand ebenso
> (F4), und die Reihenfolge ist gleichgültig, weil die Schlüssel längst lokal
> liegen (F7).
>
> **Der unangenehme Fall.** Der Rechner mit dem Auswertungsprozess wird
> kompromittiert. Sein Schlüssel wird **sofort** stillgelegt, ohne Auslieferung
> und ohne Fenster (F6). Der Sicherungsschlüssel bleibt unberührt — und weil er
> eine andere Aufgabe trägt, war der gesamte Bestand von diesem Rechner aus nie
> erreichbar.

**Referenz-Implementierung: entfällt, weil noch keine existiert (Status:
Entwurf).** Das Verfahren ist vollständig entschieden und spezifiziert; die
Skripte sind noch nicht gebaut. Die Aussagen dieses Dossiers sind damit
**durchdacht, nicht erprobt** — insbesondere die Behauptung aus F7, dass die
Reihenfolge sich von selbst ergibt, ist bisher ein Argument.

## 9. Offene Fragen

**Q1 · Wie lang ist das richtige Fenster?** F6 nennt keine Zahl. Zu kurz, und
ein langsamer Übertragungsweg lässt es zufallen, bevor die letzte Komponente
dran war; zu lang, und es steht offen, während niemand hinsieht. Vermutlich
hängt es an der Dauer des längsten Laufs — belegt ist das nicht.

**Q2 · Was, wenn die Reihenfolge nicht linear ist?** F7 nimmt an, dass eine
Komponente das Fenster öffnet und die anderen es nutzen. Bei zwei Komponenten,
die einander Zugänge geben müssten, gäbe es einen Kreis. Ist das ein
Konstruktionsfehler des Systems oder ein Fall, den das Konzept behandeln muss?

**Q3 · Wie sieht ein turnusmäßiger Wechsel aus?** F5 schafft mit `--force` die
Möglichkeit, ein Systemgeheimnis zu ersetzen, sagt aber nichts über den Ablauf:
Wie tauscht man einen Schlüssel, ohne die Komponenten auszusperren, die ihn
noch benutzen? Ein Nebeneinander von altem und neuem Wert ist nirgends
vorgesehen.

**Q4 · Trägt die Trennung „Skript versioniert, Instanz nicht" auch, wenn ein
Team ausliefert?** Hier wird durchgehend ein Betreiber angenommen. Bei mehreren
müssten die Instanzverzeichnisse geteilt werden — und dann sind sie wieder ein
gemeinsamer Ort mit Geheimnissen, für den dieses Konzept keine Antwort hat.

## 10. Quellen & Verwandtes

*Zur Fertigstellung: Fundstellen prüfen und mit Abrufdatum versehen.*

- **Vertiefung, nicht Voraussetzung — was eingerichtet wird:** Das
  Schwester-Dossier `invitation-instead-of-login`
  (<https://github.com/schlingensiepen/proposals/tree/main/invitation-instead-of-login>)
  beschreibt das Zugriffsmodell, dessen Einrichtung F6 hier auslöst: Menschen
  bekommen Identität, Maschinen Fähigkeit, und Maschinenzugänge entstehen nur im
  Fenster. Dort ist es die **Regel**, hier der **Ablauf**. Dieses Dossier ist
  ohne jenes vollständig — es setzt nur voraus, dass es überhaupt Zugänge
  einzurichten gibt, nicht welcher Art.
- **Ein zweites Beispiel für dasselbe Muster:** Das Verfahren ist nicht an ein
  bestimmtes Zugriffsmodell gebunden. Richtet die Anwendung stattdessen einen
  Verwalter mit Benutzername und Passwort ein, ändert sich an F1 bis F10 nichts:
  Das Passwort wäre dann ein Systemgeheimnis nach F2 — vom Skript erzeugt, nie
  ausgedacht, einmal im Terminal genannt und neben der Instanzkonfiguration
  gesichert. Nur der Inhalt des Fensters wäre ein anderer.
