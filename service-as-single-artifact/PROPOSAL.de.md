---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: service-as-single-artifact
language: de (führend)
question: "Ich will eine Funktion ins Netz stellen und baue dafür einen kleinen Dienst — welche Regeln nehme ich als Grundlage, damit er klein bleibt und trotzdem sicher zu betreiben ist?"
thesis: "Ein Dienst trägt genau eine Funktion, wird als ein Artefakt mit mehreren Modi gebaut und als ein Erzeugnis ausgeliefert; die Kommandos konfigurieren die Funktion anwendungsspezifisch und erzeugen für genau diese Konfiguration den Zugang — damit entfallen Rollen, Rechteverwaltung und Admin, bevor sie entstehen."
---

# Der Dienst als ein Artefakt

**In einem Satz:** Wer eine Funktion ins Netz stellen will, baut dafür einen Dienst, der genau diese eine Funktion trägt, als *ein* Artefakt mit mehreren Modi — laufen und Kommandos —, liefert genau ein Erzeugnis aus und lässt die Kommandos die Funktion anwendungsspezifisch konfigurieren und im selben Handgriff den Zugang dazu erzeugen.

**Kurzthese:** Etwas soll ins Netz — eine Funktion, die andere Menschen oder deren Agenten benutzen können sollen —, und dafür baut man schnell einen kleinen Dienst. Ebenso schnell wächst um ihn herum ein zweites Ding: ein Admin-Skript, eine Verwaltungsoberfläche, ein Debug-Endpunkt, ein zweites Image; und mit der zweiten Funktion kommen Rollen, Rechte und ein Admin. Dieses Dossier ist der Startsatz dagegen: elf Regeln, die man übernimmt, bevor die erste Zeile steht — eine Funktion, ein Artefakt, ein Erzeugnis, ein Zugang, der zusammen mit seiner Konfiguration entsteht.

## 1. Problem

Man hat eine Funktion, die ins Netz soll — etwas, das andere Menschen oder deren Agenten benutzen können sollen. Der Autor stand vor genau diesem Fall: Entwürfe sollten sich per IMAP in Postfächern ablegen lassen, aufgerufen von außen. Für so etwas baut man keinen Konzern-Dienst, sondern ein kleines Programm — und möchte es in Betrieb nehmen, bevor die Lust vergeht.

Dann kommen die Fragen, die bei jedem noch so kleinen Dienst nach wenigen Tagen kommen: Wie legt man einen Zugang an? Wer darf ihn benutzen, und wofür genau? Wie prüft man, ob die Konfiguration stimmt, *bevor* man neu startet? Wie sieht man, was anliegt? Wie räumt man auf? Und wie kommt TLS davor?

Der Reflex lautet: noch ein Werkzeug. Ein kleines Admin-Skript, das in dieselbe Ablage schreibt. Eine Weboberfläche für die Verwaltung. Ein Debug-Endpunkt, „nur intern". Ein zweites, schlankes Image für Wartungsaufgaben. Jedes dieser Dinge sieht für sich vernünftig aus, und zusammen machen sie aus dem kleinen Dienst ein Projekt — denn jedes ist eine zweite Implementierung desselben Wissens: Es muss das Konfigurationsformat kennen, die Ablagestruktur, die Prüfregeln, die Version. Ab dann altern zwei Dinge nebeneinander, und irgendwann schreibt das Werkzeug aus Fassung 1.2 etwas, das der Dienst aus Fassung 1.5 anders liest.

Der zweite Reflex ist stiller und teurer: noch eine Funktion. Wenn der Dienst schon läuft, kann er das Nachbarproblem doch gleich mitlösen. Von diesem Moment an gibt es Aufrufer, die das eine dürfen sollen und das andere nicht — und damit Rechte, Rollen, eine Verwaltung dafür und jemanden, der sie führt. Der Dienst, der als Nachmittagsprojekt begann, hat jetzt ein Berechtigungskonzept.

Drei Rollen tragen das Problem, und bei einem kleinen Dienst fallen die ersten beiden oft in einer Person zusammen — das ist der Regelfall, für den dieses Dossier geschrieben ist. **Der Erbauer** schreibt beides und merkt die Doppelung zuerst beim dritten Konfigurationsfeld, das er an zwei Stellen nachziehen muss. **Der Betreiber** muss zwei Erzeugnisse ausliefern, zwei Versionen im Blick behalten und im Störfall wissen, welches von beiden gerade recht hat. **Der Übernehmende** findet Monate später ein Werkzeug vor, dessen Verhältnis zum Dienst niemand mehr erklären kann — und traut sich nicht, es zu benutzen.

Der übliche Ausweg — Disziplin, gemeinsame Bibliotheken, ein Monorepo — mildert die Doppelung, hebt sie aber nicht auf: Zwei Erzeugnisse bleiben zwei Erzeugnisse, und sie werden getrennt gebaut, getrennt ausgeliefert und getrennt vergessen. Die strukturelle Antwort setzt eine Ebene tiefer an: Was nicht getrennt existiert, kann nicht getrennt altern — und was gar nicht erst mehrdeutig ist, braucht keine Rechteverwaltung.

Dieses Dossier ist deshalb keine Architekturlehre, sondern eine **Grundlage zum Losbauen**: ein Vorgehen in elf Schritten (§2) und elf Regeln, die es tragen (§4) — beides übernimmt man, bevor die erste Zeile steht. Wer so anfängt, hat die Entscheidungen, die man sonst nach und nach und meist unter Zeitdruck trifft, schon getroffen — und spart sich genau die Nebenerzeugnisse, die den kleinen Dienst groß machen.

## 2. Das Konzept

Alles beginnt bei der Funktion: **Ein Dienst trägt genau eine.** Sie ist sein Zweck, in einem Satz beschreibbar — „eine Mail in einen IMAP-Ordner einstellen". Die Netzfläche zerlegt sie in Methoden: eine Hauptmethode und die **Hilfsmethoden**, ohne die sie nicht geht. Beim Beispiel ist der Datei-Upload keine zweite Funktion, sondern der Vorbereitungsschritt für Anhänge; dazu kommen die Statusabfrage zum eingereichten Vorgang und das Aufräumen des Hochgeladenen. Alle Methoden dienen derselben Funktion und teilen denselben Zugang — es gibt nichts, was der eine Aufrufer dürfte und der andere nicht.

Daraus folgt die Bauform: **ein Artefakt mit mehreren Modi.** Dasselbe ausführbare Programm läuft als Dienst und führt die Kommandos aus — prüfen, Zustand zeigen, konfigurieren, Zugang ausstellen, aufräumen; welcher Modus gilt, entscheidet der Aufruf. Braucht der Dienst weitere Prozessarten, etwa einen Arbeiter für eine Warteschlange, ist auch das ein Modus und kein zweites Programm. Ausgeliefert wird **ein Erzeugnis**: ein Image beziehungsweise ein Binary, gegen das auch die Kommandos laufen, in derselben Umgebung, mit derselben Version. Beide Modi lesen **dieselbe Konfiguration** mit demselben Code, und das Prüfkommando fällt genau das Urteil, das der Start fällen würde. Was den Neustart überdauern muss, liegt **außerhalb** des Artefakts an benannten Orten.

Der Kern des Zuschnitts steckt aber in dem, was die Kommandos tun: **Sie konfigurieren die Funktion anwendungsspezifisch und erzeugen für genau diese Konfiguration den Zugang.** Der Dienst kann „eine Mail in einen IMAP-Ordner einstellen"; das Kommando legt fest, in *welches* Postfach und in *welchen* Ordner — und stellt im selben Handgriff das Zugangsartefakt aus, das genau diese Ausprägung verkörpert. Der Aufrufer wählt nie, was die Konfiguration festgelegt hat; er ruft die Funktion auf, und wohin sie wirkt, steht nicht in seiner Anfrage.

Das ist der Grund, warum der Rest so klein ausfällt. Es gibt keinen Moment, in dem jemand einem bestehenden Zugang Rechte *gibt* — die Vergabe ist die Konfiguration, und der Zugang ist ihr Griff. Also braucht dieser Dienst **kein Rechtemodell, keine Rollen und keinen Admin**: Verwaltung ist Konfiguration, und wer die Konfigurationsdatei ändern darf, verwaltet. Entsprechend hat der **laufende Dienst keine Verwaltungsfläche** — keinen Verwaltungspfad, keine Debug-Route —, und der **Rechte-Schnitt läuft zwischen den Modi**: Der Dienstmodus arbeitet mit den engsten Rechten, die zum Laufen reichen, geschrieben wird nur im Kommando-Modus, durchgesetzt über Benutzer und Dateirechte.

Das Prinzip ist nicht neu; es steht in der Ahnenreihe der Zwölf-Faktoren-Regel, dass Verwaltungsprozesse in identischer Umgebung wie der Dienst laufen und ihr Code mit dem Anwendungscode ausgeliefert werden muss (§10). Neu ist die Konsequenz, mit der hier daraus ein Zuschnitt wird: nicht „Admin-Code liegt beim Anwendungscode", sondern „Admin-Code *ist* der Anwendungscode, in einem anderen Modus" — und die Verbindung mit der einen Funktion, die das Rechtethema von vornherein auflöst. Für den, der eine Funktion ins Netz stellen will, ist das der ganze Aufwand: ein Programm mit ein paar Kommandos mehr, ein Erzeugnis, eine Konfigurationsdatei, ein Verzeichnis daneben, ein Proxy davor.

### Das Vorgehen

Die Festlegungen in §4 sagen, was gelten muss. Dieser Abschnitt sagt, in welcher Reihenfolge man vorgeht und was am Ende jedes Schrittes vorliegt — das ist der eigentliche Gebrauchswert für den, der loslegen will. Die Schritte 0 bis 3 sind eine halbe Stunde Denkarbeit und entscheiden alles Weitere; wer sie überspringt, holt sie später unter schlechteren Bedingungen nach.

**0 · Prüfen, ob es überhaupt ein Dienst sein muss.** Wer die Funktion aufruft, sitzt entweder auf demselben Rechner oder anderswo. Sitzt er dort — ein Skript, ein Zeitplaner, ein Agent auf derselben Maschine —, dann ist die Antwort ein Kommandozeilenwerkzeug und kein Dienst: Die Zugangskontrolle leisten die Rechte des Betriebssystems, und Netzfläche, Zugangsartefakte und Proxy entfallen ersatzlos. Erst wenn der Aufrufer über das Netz kommt oder später kommen soll, lohnt der Rest dieses Vorgehens. *Ergebnis:* eine begründete Entscheidung. Fällt sie gegen den Dienst, sind die Schritte 1 bis 3 trotzdem nützlich und der Rest überflüssig.

**1 · Den Satz schreiben.** Ein Satz, der sagt, was der Dienst tut — ohne „und außerdem". Gelingt er nicht, ist der Zuschnitt noch nicht gefunden, und alles Weitere wäre verfrüht. *Ergebnis:* der Satz. Er wird später die Kurzbeschreibung des Dienstes, die erste Zeile seiner Kommandohilfe und der Eintrag, unter dem ihn jemand wiederfindet. (F1)

**2 · Die Methoden ableiten.** Aus dem Satz folgt die Hauptmethode; dazu kommen die Hilfsmethoden, ohne die sie nicht geht — Vorbereitung (etwa ein Upload), Statusabfrage zum Vorgang, Aufräumen des Vorbereiteten. *Ergebnis:* die Methodenliste. Was sich hier nicht unterordnen lässt, gehört nicht in diesen Dienst: Es wird gestrichen oder ein zweiter Dienst. (F1)

**3 · Die Ausprägung bestimmen.** Was ist von Anwendungsfall zu Anwendungsfall verschieden — Zielsystem, Ablageort, Grenzen? Diese Frage ist der Angelpunkt des ganzen Zuschnitts: Ihre Antwort ist genau das, was später der Konfigurationseintrag trägt, was der Zugang verkörpert und was der Aufrufer folglich nie wählt. *Ergebnis:* die Feldliste der Ausprägung. (F9)

**4 · Die Konfiguration festlegen.** Eine Datei mit den globalen Einstellungen und einer Liste der Ausprägungen aus Schritt 3; Geheimnisse stehen nicht darin, sondern als Verweis auf Umgebungsvariable oder Datei. Dazu die Fundort-Präzedenz und die Regel, dass jedes Kommando anzeigt, welche Datei es benutzt. *Ergebnis:* Konfigurationsschema und Fundort-Regel. (F5)

**5 · Die Zustandsorte benennen.** Was muss den Neustart überdauern — Daten, Warteschlangen, Hochgeladenes? Jeder dieser Orte bekommt einen benannten Platz außerhalb des Artefakts, sinnvollerweise je Ausprägung getrennt. *Ergebnis:* das Verzeichnislayout. (F6)

**6 · Den Kommandobaum aufsetzen — vor der Fachlogik.** Mindestens: Dienst starten · Konfiguration prüfen (optional mit Verbindungstest gegen das Zielsystem) · Zustand zeigen · eine Ausprägung einrichten, ändern, entfernen · Version. Das Einrichten einer Ausprägung stellt zugleich den Zugang aus — ein Kommando, nicht zwei (F9); ein eigenes Kommando „Zugang ausstellen" gibt es bewusst nicht, wohl aber eines zum Erneuern eines bestehenden Zugangs. Dazu die Exit-Codes und die maschinenlesbare Ausgabe. Die Reihenfolge ist Absicht: Mit fertigem Kommandogerüst lässt sich alles Weitere über Kommandos prüfen, statt es über die Netzfläche testen zu müssen. *Ergebnis:* das lauffähige Gerüst, das noch nichts kann außer sich selbst zu erklären. (F2, F3, F9)

**7 · Den Dienstmodus bauen.** Die Netzfläche bietet genau die Methoden aus Schritt 2; die Zugangsprüfung schlägt in der Konfiguration nach, was der vorgelegte Zugang verkörpert; es gibt keinen Verwaltungspfad und höchstens einen Bereitschaftspunkt, der nichts als „läuft" sagt. *Ergebnis:* der Dienst, gegen eine von Hand geschriebene Konfiguration lauffähig. (F1, F7)

**8 · Das Einrichtungskommando fertigstellen.** Das in Schritt 6 angelegte Kommando bekommt jetzt seine Wirkung: Es konfiguriert eine Ausprägung und erzeugt in demselben Handgriff den Zugang dazu — Zugangsartefakt genau einmal anzeigen, davon nur den Hash und eine harmlose Kurzkennung behalten, Rotation als eigenes Kommando. *Ergebnis:* der Weg von „Dienst läuft" zu „jemand darf ihn benutzen" — ohne dass ein Geheimnis irgendwo liegen bleibt. (F9)

**9 · Paketieren und die Auslieferung einrichten.** Ein Erzeugnis bauen, dessen Einstiegspunkt der Dienstmodus ist und gegen das die Kommandos in derselben Umgebung laufen. Hier entscheidet sich mehr, als es aussieht — und wer den Dienst *für jemanden* baut, etwa als Agent für seinen Anwender, **fragt an dieser Stelle ausdrücklich nach, statt zu wählen**: Soll ein Container-Image bereitgestellt werden, oder genügt für die Zielumgebung das Paket beziehungsweise das Binary? Soll die Bauleitung gleich mit eingerichtet werden, sodass das Image bei jeder Fassung automatisch gebaut und veröffentlicht wird? Und soll die Ausrolldatei dann auf dieses veröffentlichte Image zeigen — was zugleich klärt, ob die Zielmaschine die Registry erreicht und, bei nicht öffentlicher Quelle, mit welchen Zugangsdaten. Die drei Fragen kosten eine Minute und sind später teuer: Der Wechsel von „baut lokal" auf „zieht das veröffentlichte Image" fasst jede Ausrollstelle noch einmal an. *Ergebnis:* das Artefakt und ein Auslieferungsverzeichnis — Container-Beschreibung, Ausrolldatei, Beispiel für den Reverse Proxy, Einheit für die Prozessform und, falls gewünscht, die Bauleitung. (F2, F4)

**10 · Die Betriebsform herstellen.** Lokaler Endpunkt, Reverse Proxy davor mit dem öffentlichen Namen und dem Zertifikat; getrennte Benutzer und Dateirechte, sodass der Dienstprozess liest, wo die Kommandos schreiben. *Ergebnis:* der Dienst ist erreichbar, und der Rechte-Schnitt steht in der Prozessform statt in guten Vorsätzen. (F8, F11)

Was dieses Vorgehen bewusst nicht vorgibt: Sprache, Rahmenwerk, Datenhaltung, Schnittstellendialekt. Es legt die Reihenfolge und die Ergebnisse fest, nicht die Mittel.

## 3. Begriffe

**Funktion** — das eine, was der Dienst kann, in einem Satz beschreibbar; sein Zweck.

**Hauptmethode** — der Aufruf, der die Funktion auslöst.

**Hilfsmethode** — ein Aufruf, ohne den die Funktion nicht geht oder nicht überprüfbar ist: Vorbereitung (etwa Datei-Upload), Statusabfrage, Aufräumen. Hilfsmethoden sind keine eigenen Funktionen und teilen den Zugang der Hauptmethode.

**Artefakt** — das eine gebaute, versionierte, auslieferbare Erzeugnis eines Dienstes: das Binary beziehungsweise das Container-Image.

**Modus** — eine Betriebsart desselben Artefakts, gewählt beim Aufruf.

**Dienstmodus** — der lange laufende Modus, der die Netzfläche bedient.

**Kommando** — ein Einmal-Aufruf desselben Artefakts für Konfiguration, Prüfung, Zustandsanzeige oder Aufräumen; lokal gestartet, endet nach der Aufgabe.

**Zugang** — das Artefakt, mit dem ein Aufrufer die Funktion benutzt (in der Regel ein Token); es verkörpert eine konkrete, vorkonfigurierte Ausprägung der Funktion.

**Verwaltungsfläche** — jede über das Netz erreichbare Möglichkeit, den Dienst zu konfigurieren, zu inspizieren oder Zugänge zu ändern; in diesem Konzept nicht vorgesehen.

**Zustand** — alles, was einen Neustart überdauern muss: Daten, Konfiguration, Geheimnisse, Warteschlangen; liegt außerhalb des Artefakts.

**Prozessform** — wie das Artefakt gestartet und beaufsichtigt wird: systemd-Unit, Container-Laufzeit, etwas anderes; für dieses Konzept austauschbar.

**Reverse Proxy** — die vorgeschaltete Instanz, die öffentliche Namen und TLS bedient und an den lokalen Endpunkt des Dienstes weiterreicht.

**Prüfkommando** — das Kommando, das die Konfiguration bewertet, ohne den Dienst zu starten.

**Versionsdrift** — der Zustand, in dem zwei getrennt gebaute Erzeugnisse desselben Systems verschiedene Fassungen desselben Wissens tragen.

## 4. Festlegungen

**Landkarte.** Elf Festlegungen in drei Blöcken. **F1–F6** beschreiben *den Zuschnitt*: die eine Funktion, das eine Artefakt mit seinen Modi, den Vertrag der Kommandos, das eine Erzeugnis, die eine Konfiguration und den Zustand daneben. **F7–F10** ziehen *die Trennung*: keine Verwaltungsfläche im laufenden Dienst, ein Rechte-Schnitt zwischen den Modi, ein Zugang, der mit seiner Konfiguration entsteht — und deshalb gar keine Verwaltungsrechte. **F11** betrifft *die Außenkante* im Betrieb. F1 trägt alles: Mit der zweiten Funktion kehren Rechte, Rollen und Verwaltung zurück, und F9 wie F10 verlieren ihren Boden. Jede Festlegung nennt, für wen sie gilt.

### F1 · Ein Dienst, eine Funktion

**Gilt für:** Erbauer.

**Was gilt.** Der Dienst trägt genau eine Funktion; sie ist sein Zweck und in einem Satz beschreibbar. Die Netzfläche zerlegt sie in Methoden: eine Hauptmethode und die Hilfsmethoden, ohne die sie nicht geht (Vorbereitung, Statusabfrage, Aufräumen des Vorbereiteten). Alle Methoden dienen derselben Funktion und teilen denselben Zugang. Wer eine zweite, unabhängige Funktion braucht, baut einen zweiten Dienst.

**Warum.** Die eine Funktion ist die Voraussetzung dafür, dass alles Weitere klein bleibt: Ein Dienst mit genau einer Funktion braucht keine Unterscheidung, wer welche Methode darf — und damit weder Rollen noch Rechteverwaltung noch jemanden, der sie führt (F9, F10). Er ist außerdem in einem Satz beschreibbar, für Menschen wie für Agenten, und seine Beschreibung bleibt lesbar, statt zum Katalog zu werden.

**Naheliegend und verworfen.** *Ein Dienst für alles („wenn er schon läuft")* — sobald zwei unabhängige Funktionen unter einem Zugang liegen, braucht man eine Rechteunterscheidung, und mit ihr Rollen und deren Verwaltung; die Ersparnis einer Auslieferung wiegt diese Last nicht auf. *Ein Zugang je Methode* — dann jongliert der Aufrufer mehrere Zugänge, um eine Datei hochzuladen und sie zu verwenden; die Fähigkeit ist die Funktion, nicht der einzelne Aufruf. *Die Hilfsmethoden als eigene kleine Dienste* — verteilt einen Arbeitsablauf auf mehrere Auslieferungen und erzeugt genau die Abstimmungsprobleme, die dieses Dossier vermeidet.

**Woran man merkt, dass es verletzt ist.** Man diskutiert, ob ein Aufrufer die eine Methode dürfen soll und die andere nicht — oder der Dienst lässt sich nicht mehr in einem Satz beschreiben, ohne „und außerdem" zu sagen.

### F2 · Ein Artefakt, mehrere Modi

**Gilt für:** Erbauer.

**Was gilt.** Dienst und Kommandos sind dasselbe ausführbare Artefakt; der Modus wird beim Aufruf gewählt. Vorhanden sind mindestens: der **Dienstmodus**; ein **Prüfkommando**, das die Konfiguration bewertet, ohne zu starten; ein **Zustandskommando**, das zeigt, was anliegt; **Konfigurationskommandos** zum Einrichten und Ändern; und das Kommando, das einen **Zugang ausstellt**. Weitere Prozessarten (Arbeiter, Zeitplaner) sind weitere Modi desselben Artefakts. Ein separates Verwaltungswerkzeug wird nicht gebaut.

**Warum.** Die Kommandos teilen mit dem Dienst alles Wesentliche: Konfigurationsformat, Ablagestruktur, Prüfregeln, Datenschema, Version. Ein zweites Werkzeug ist eine zweite Implementierung dieses Wissens — und zwei Implementierungen desselben Wissens driften, sobald sich eine ändert. Im selben Artefakt können sie es nicht. Der Mindest-Satz ist ausdrücklich genannt, weil der Zuschnitt ohne ihn eine Haltung bliebe: Diese fünf Dinge braucht jeder Dienst, der länger als eine Woche läuft.

**Naheliegend und verworfen.** *Ein eigenes kleines Verwaltungswerkzeug* — angeblich sauberer, tatsächlich ein zweiter Konfigurations-Parser mit eigener Versionsnummer. *Eine Verwaltungs-Weboberfläche* — bequem, aber eine dauerhaft offene Angriffsfläche mit eigener Anmeldung (F7). *Skripte, die direkt in die Ablage schreiben* — umgehen jede Prüfung, die der Dienst beim Schreiben anwendet, und kennen die Struktur nur vom Hörensagen.

**Woran man merkt, dass es verletzt ist.** Verwaltungswerkzeug und Dienst tragen verschiedene Versionsnummern — oder ein neues Konfigurationsfeld muss an zwei Stellen nachgezogen werden.

### F3 · Kommandos sind skriptbar

**Gilt für:** Erbauer.

**Was gilt.** Jedes Kommando endet mit einem Exit-Code, dessen Bedeutung festgelegt und dokumentiert ist; unterschieden werden mindestens: alles gut · Laufzeitfehler · falscher Aufruf · Konfiguration ungültig. Kommandos, die auflisten oder Zustand zeigen, bieten zusätzlich eine maschinenlesbare Ausgabe an. Kein Kommando verlangt eine interaktive Eingabe, die es nicht auch als Argument entgegennimmt.

**Warum.** Verwaltung ohne Oberfläche muss automatisierbar sein — sonst entsteht das Zweitwerkzeug durch die Hintertür: als Sammlung von Skripten, die Textausgaben zerlegen und bei jeder Formatänderung stillschweigend brechen. Exit-Codes sind der billigste Vertrag, den ein Programm anbieten kann, und der einzige, den eine Ausrollkette ohne Zutun versteht.

**Naheliegend und verworfen.** *Nur Text ausgeben („kann man ja durchsuchen")* — macht jede Formatverbesserung zum Bruch fremder Automatisierung, und zwar zu einem lautlosen. *Interaktive Rückfragen ohne nicht-interaktiven Weg* — dann hängt das Kommando in jeder unbeaufsichtigten Umgebung. *Alles über einen Sammel-Exit-Code* — der Unterschied zwischen „falsch aufgerufen" und „Konfiguration kaputt" ist genau der, den ein Betreiber um drei Uhr nachts braucht.

**Woran man merkt, dass es verletzt ist.** In der Ausrollkette steht eine Textsuche auf einer Kommandoausgabe — oder ein Kommando wartet in einer unbeaufsichtigten Umgebung auf eine Eingabe.

### F4 · Ein Erzeugnis, eine Auslieferung

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Es wird genau ein Auslieferungserzeugnis gebaut und veröffentlicht. Kommandos laufen gegen dieses Erzeugnis in derselben Umgebung wie der Dienst — im Container-Betrieb als Einmal-Aufruf gegen dasselbe Image, auf dem Host als dasselbe Binary. Ein zweites, „schlankes" Verwaltungs-Image gibt es nicht. Zum Erzeugnis gehört, **wie es entsteht und wie es ankommt**: eine Bauleitung, die es reproduzierbar aus der Quelle erzeugt und veröffentlicht, und eine Ausrolldatei, die genau dieses veröffentlichte Erzeugnis **benennt**, statt es auf der Zielmaschine neu zu bauen. Wird doch lokal gebaut — in der Entwicklung, oder weil die Zielmaschine keine Registry erreicht —, ist das eine benannte Ausnahme und im Auslieferungsverzeichnis als solche gekennzeichnet.

**Warum.** Ein zweites Erzeugnis kann eine andere Version haben, andere Bibliotheken, andere Pfade, eine andere Zeitzone — und dann sieht das Kommando etwas anderes als der Dienst. Die identische Umgebung ist keine Bequemlichkeit, sondern die Voraussetzung dafür, dass ein Prüfergebnis überhaupt etwas über den Dienst aussagt. Dasselbe Argument trägt bis in die Ausrolldatei: Wer dort lokal bauen lässt, hat am Ende so viele Erzeugnisse wie Maschinen — jedes mit dem Stand der Grundschichten vom Tag seines Baus. „Ein Erzeugnis" ist dann nur noch eine Absicht.

**Naheliegend und verworfen.** *Ein eigenes Verwaltungs-Image ohne Dienst-Abhängigkeiten* — spart Megabyte und erkauft sie mit Versionsdrift. *Die Ausrolldatei baut lokal* — beim ersten Mal bequem, danach die Vervielfachung des Artefakts: Zwei Maschinen mit derselben Fassungsnummer können sich verschieden verhalten, und niemand sieht, warum. Als Entwicklungsbequemlichkeit in Ordnung, als Auslieferungsweg nicht. *Verwaltung als Beiwagen-Behälter* — dieselbe Trennung, nur mit mehr Bewegungsteilen. *Kommandos vom Rechner des Entwicklers gegen die Produktionsablage* — andere Umgebung, andere Version, und der einzige Schutz ist Aufmerksamkeit.

**Woran man merkt, dass es verletzt ist.** Jemand muss fragen, welche Version das Verwaltungswerkzeug hat — oder ein Kommando verhält sich auf dem Server anders als lokal — oder zwei Maschinen tragen dieselbe Fassungsnummer und verhalten sich verschieden, weil jede ihr Image selbst gebaut hat.

### F5 · Eine Konfiguration, ein Urteil

**Gilt für:** Erbauer.

**Was gilt.** Alle Modi lesen dieselbe Konfigurationsquelle mit demselben Code. Der Fundort folgt einer festgelegten Präzedenz — ausdrückliche Angabe beim Aufruf, dann Umgebungsvariable, dann benutzernaher Ort, dann systemweiter Ort —, und **jedes Kommando zeigt an, welche Datei es tatsächlich verwendet**. Das Prüfkommando nimmt genau das Startverhalten vorweg: Was es annimmt, startet; was es verwirft, startet nicht. Geheimnisse stehen nicht in der Datei; sie werden aus ihr heraus referenziert, und zwar in dieser Rangfolge: **1.** Umgebungsvariable — der Dienst DARF dafür einen kanonischen Namen vorschlagen; **2.** wenn die Umgebung das nicht hergibt: Datei mit engen Rechten, ausschließlich als Durchleitung benutzt — der Inhalt fließt direkt in den Aufruf und erscheint nie in Ausgaben, Logs oder Fehlermeldungen; **3.** alles Weitere (etwa ein Geheimnisverwalter der Umgebung) ist zulässig, solange es diese beiden Eigenschaften nicht unterschreitet. Ist ein Feld auf mehreren Wegen belegt, gewinnt der höhere Rang, und das Prüfkommando meldet die Mehrdeutigkeit. Das Schwester-Konzept (§10) begründet diese Rangfolge ausführlich; befolgen lässt sie sich ohne es.

**Warum.** Eine Prüfung, die nicht dieselbe Entscheidung fällt wie der Start, ist schlimmer als keine: Sie erzeugt Vertrauen, das im Störfall bricht. Die Anzeigepflicht klingt nach Kleinigkeit und ist der häufigste Zeitfresser bei der Fehlersuche — zwei Konfigurationsdateien auf einem Rechner, und man ändert zuverlässig die falsche. Und eine Datei lässt sich als Ganzes prüfen, versionieren und zurückrollen; verstreute Umgebungsvariablen nicht.

**Naheliegend und verworfen.** *Ein separates Schema-Prüfwerkzeug* — die zweite Wahrheit über die Konfiguration, siehe F2. *Alle Konfiguration über Umgebungsvariablen* — verbreitet, aber ohne Gesamtprüfung und ohne Versionierbarkeit; für Geheimnisse bleibt es der richtige Weg, für den Rest nicht. *Prüfung durch Probestart* — bindet Ports und Zustand und ist im laufenden Betrieb nicht wiederholbar.

**Woran man merkt, dass es verletzt ist.** Der Dienst startet nicht, obwohl die Prüfung grün war — oder jemand ändert lange eine Datei, die gar nicht gelesen wird.

### F6 · Der Zustand liegt außerhalb des Artefakts

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Alles, was einen Neustart überdauern muss — Daten, Konfiguration, Geheimnisse, Warteschlangen —, liegt an benannten Orten außerhalb des Artefakts. Das Artefakt ist austauschbar: Ein Wechsel der Fassung nimmt nichts mit und lässt nichts zurück.

**Warum.** Sonst ist „ein Erzeugnis" eine Behauptung: Ein Update würde Zustand vernichten, ein Rollback Daten verlieren, und niemand könnte eine Fassung gefahrlos zurücknehmen. Die Austauschbarkeit des Artefakts ist der praktische Gegenwert des ganzen Zuschnitts.

**Naheliegend und verworfen.** *Die Datenablage im Image* — bequem beim ersten Start, tödlich beim zweiten. *Konfiguration einkompilieren* — jede Änderung wird zum Neubau, jede Umgebung zum eigenen Erzeugnis (F4). *Zustand im Arbeitsverzeichnis ohne benannten Ort* — funktioniert, bis jemand die Prozessform wechselt.

**Woran man merkt, dass es verletzt ist.** Ein Fassungswechsel erfordert eine Datenwanderung — oder ein Rollback verliert etwas, das vorher da war.

### F7 · Der laufende Dienst hat keine Verwaltungsfläche

**Gilt für:** Erbauer.

**Was gilt.** Der Dienstmodus bietet keine Verwaltungs-Endpunkte, keine Debug-Routen und keine Verwaltungsoberfläche an. Prüfung, Inspektion und Änderung sind Kommandos. Diese brauchen **keine eigene Anmeldung**: Sie werden lokal von jemandem gestartet, der bereits Serverzugang hat — dieser Zugang *ist* die Authentifizierung. Nicht gemeint sind reine Betriebspunkte ohne Wirkung und ohne Auskunft über Inhalte, etwa eine Bereitschaftsanzeige, die nichts als „läuft" sagt; wo deren Grenze verläuft, ist offen (Q3).

**Warum.** Eine Verwaltungsfläche im Netz ist rund um die Uhr erreichbar, während ein Kommando nur läuft, wenn es jemand startet. Sie braucht eine eigene Anmeldung, eine eigene Rechteprüfung und eigene Fehlerbehandlung — drei neue Stellen, an denen etwas falsch sein kann, für eine Aufgabe, die ohnehin Serverzugang voraussetzt. Und sie ist der bequemste Weg für einen Angreifer, der die Netzfläche schon hat.

**Naheliegend und verworfen.** *Ein Verwaltungspfad hinter einfacher Anmeldung* — die schwächste Anmeldung des Systems bewacht dessen stärkste Wirkung. *Ein Debug-Endpunkt „nur intern"* — „intern" ist eine Netzannahme, keine Eigenschaft. *Laufzeit-Umschaltung von Verhalten über die Netzfläche* — macht den Dienst von außen konfigurierbar und die Konfigurationsdatei zur Fiktion (F5).

**Woran man merkt, dass es verletzt ist.** In der Reverse-Proxy-Konfiguration muss ein Pfad ausdrücklich gesperrt werden — oder eine Sperre fehlt, und niemand merkt es.

### F8 · Der Rechte-Schnitt läuft zwischen den Modi

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Der Zustand zerfällt in zwei Arten, und der Rechte-Schnitt folgt dieser Teilung. **Konfiguration** — was der Dienst tun soll und für wen — wird ausschließlich von den Kommandos geschrieben; der Dienstmodus liest sie nur. **Betriebsdaten** — Warteschlangen, Vorgangsstände, Zwischenablagen, alles, was im Vollzug der Funktion entsteht — schreibt der Dienstmodus selbst; die Kommandos dürfen dort ebenfalls schreiben, denn Aufräumen und Eingreifen sind ihre Aufgabe. Der Dienstmodus läuft im Übrigen mit den engsten Rechten, die zum Laufen reichen. Durchgesetzt wird beides über getrennte Benutzer und Dateirechte in der Prozessform, nicht über Absicht im Code: Konfigurationsort und Betriebsdatenort sind verschiedene Verzeichnisse mit verschiedenen Rechten. Welche Rechte je Modus genau nötig sind, hängt vom Dienst ab und ist bei der Übernahme festzulegen (Q1).

**Warum.** Eine kompromittierte Netzfläche erbt exakt die Rechte des Dienstprozesses. Kann der die **Konfiguration** nicht schreiben, kann der Angreifer sie auch nicht — er kann die Funktion missbrauchen, aber nicht umwidmen, keine Zugänge ausstellen und kein Ziel verschieben; aus der Übernahme wird ein begrenzter Schaden. Auf die **Betriebsdaten** trifft das nicht zu, und es wäre auch sinnlos: Der Dienst muss dort schreiben, um seine Arbeit zu tun. Die Trennung ist deshalb keine Härtung gegen alles, sondern eine gegen die Umwidmung — und genau die ist der teure Fall. Im Code durchgesetzte Trennung hilft dabei nicht: Wer den Prozess kontrolliert, kontrolliert auch dessen Prüfungen.

**Naheliegend und verworfen.** *Beide Modi als Systemverwalter* — bequem, und der Zuschnitt ist wertlos. *Ein Dienstbenutzer mit Vollzugriff auf alles* — dieselbe Bequemlichkeit, nur mit besserem Gewissen; damit fällt die Trennung zwischen Konfiguration und Betriebsdaten in sich zusammen. *Konfiguration und Betriebsdaten im selben Verzeichnis* — dann lassen sich die Rechte nicht mehr unterscheiden, ohne jede Datei einzeln zu verwalten. *Trennung nur im Code („der Dienst ruft die Schreibfunktion nie auf")* — eine Absichtserklärung, die ein Angreifer nicht liest.

**Woran man merkt, dass es verletzt ist.** Der Dienstprozess darf die Konfigurationsdatei schreiben — oder Konfiguration und Betriebsdaten liegen so beieinander, dass niemand sagen kann, wer was ändern darf.

### F9 · Konfiguration und Zugang entstehen in einem Akt

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Die Kommandos konfigurieren die Funktion **anwendungsspezifisch** — welches Zielsystem, welche Ablage, welche Grenzen — und erzeugen für **genau diese Konfiguration** den Zugang; beides ist ein Handgriff. Was die Konfiguration festlegt, wählt der Aufrufer nie: Der Zugang trägt es. Einen Zugang ohne zugehörige Konfiguration gibt es nicht, und eine Änderung, die die Ausprägung verschiebt, ist ein neuer Zugang.

**Warum.** Hier verschwindet die Rechtefrage, bevor sie entsteht. Wer konfiguriert, vergibt; was vergeben wurde, steht in der Konfiguration; der Zugang ist nur der Griff daran. Es gibt keinen Zeitpunkt, an dem jemand einem bestehenden Zugang etwas *hinzufügt* — und damit auch nichts zu verwalten (F10). Nebenbei entfällt eine Prüfung im Dienst: Der Aufrufer kann das Ziel gar nicht benennen, also muss niemand prüfen, ob er es dürfte. **Das gilt ausdrücklich auch im Alleinbetrieb**, wo Betreiber und Aufrufer dieselbe Person sind und ein Zugangsartefakt zunächst überflüssig wirkt: Es kostet dort nichts, weil es beim Konfigurieren ohnehin mit entsteht — es gibt keinen Schritt, den man sich sparen könnte. Nachrüsten dagegen ist teuer: Dann sind bestehende Aufrufer umzustellen, Ziele nachträglich an Zugänge zu binden, und eine Ausprägung ist zu erfinden, wo bisher alles global war — und das geschieht erfahrungsgemäß genau dann, wenn der zweite Anwender dazukommt und keine Zeit ist. Von Anfang an eingebaut skaliert der Zuschnitt nach oben, ohne dass je umgebaut werden müsste: Der Schritt vom ersten zum zweiten Aufrufer ist dann ein einziger Kommandoaufruf.

**Naheliegend und verworfen.** *Erst den Zugang erzeugen, dann konfigurieren* — schafft den Zwischenzustand „Zugang ohne Zuschnitt", und genau in diesem Zwischenraum entsteht Rechteverwaltung. *Das Ziel zur Laufzeit wählbar machen („der Aufrufer sagt, wohin")* — verlagert die Zuständigkeit an den Aufrufer und zwingt den Dienst zu einer Prüfung, die er ohne diese Wahlmöglichkeit nicht bräuchte. *Selbstregistrierung mit nachträglicher Freigabe* — sinnvoll für Dienste, die Agenten ohne Betreiber-Handgriff aufnehmen wollen (Schwester-Konzept, §10); hier unnötig, weil der Betreiber ohnehin konfiguriert, und teuer, weil sie genau den Zwischenzustand einführt.

**Woran man merkt, dass es verletzt ist.** Man diskutiert, welche Rechte ein bestehender Zugang bekommen soll.

### F10 · Der Dienst kennt keine Verwaltungsrechte

**Gilt für:** Erbauer.

**Was gilt.** Es gibt keinen Verwaltungsbenutzer, keinen Verwaltungs-Zugang und kein Rechtemodell für die Verwaltung. Verwaltung ist Konfiguration, und wer die Konfiguration ändern darf, verwaltet — durchgesetzt über Dateirechte (F8). Kein ausgestellter Zugang trägt jemals Verwaltungsmacht; er trägt die Funktion, sonst nichts. Nicht gemeint ist das fachliche Modell eines Dienstes, dessen *Funktion* die Verwaltung von Konten oder Freigaben ist: Was er als Funktion anbietet, bietet er über seine Netzfläche an (F1).

**Warum.** Ein Rechtemodell ist teuer — es will entworfen, geprüft, dokumentiert und im Betrieb gepflegt werden —, und für einen Dienst mit einer Funktion und vorkonfigurierten Zugängen hat es keinen Gegenstand. Wo es keine Verwaltungsrechte gibt, kann sie niemand erben, ausweiten oder zu entziehen vergessen.

**Naheliegend und verworfen.** *Ein Verwaltungs-Token „für Notfälle"* — genau das Artefakt, dessen Verlust alles kostet, und es liegt garantiert irgendwo. *Rollen im Dienst für Betriebsaufgaben* — führt die Rechteverwaltung ein, die dieser Zuschnitt gerade spart, und macht die Netzfläche wieder zum Verwaltungsweg (F7).

**Woran man merkt, dass es verletzt ist.** Es existiert ein Zugang, mit dem man mehr kann als die eine Funktion.

### F11 · Kein TLS im Dienst, lokaler Endpunkt

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Der Dienst lauscht auf einem lokalen Endpunkt; öffentliche Namen, Zertifikate und TLS bedient ein Reverse Proxy davor. Die Prozessform — systemd-Unit oder Container-Laufzeit — ist frei wählbar; das Artefakt bleibt dasselbe.

**Warum.** Zertifikatsbeschaffung und -erneuerung sind gelöste Probleme, aber nicht in jedem Dienst neu: Ein eingebauter Zertifikatsclient bindet Erneuerungen an Neustarts des Dienstes und macht jede Fassung zum Sicherheitsereignis. Und ein Dienst, der nur lokal lauscht, hat ohne Proxy überhaupt keine Außenfläche — die Freigabe ist eine bewusste Handlung, kein Standardzustand.

**Naheliegend und verworfen.** *TLS im Dienst* — eigener Zertifikatsclient, eigene Erneuerungslogik, Neustarts bei jedem Zertifikatswechsel. *Direkt auf allen Schnittstellen lauschen* — die Freigabe wird zum Versehen statt zur Entscheidung.

**Woran man merkt, dass es verletzt ist.** Der Dienst muss neu gestartet werden, weil ein Zertifikat erneuert wurde.

**Tragwerk (Kurzrekapitulation).** F1 trägt alles: Mit einer zweiten unabhängigen Funktion kehren Rechteunterscheidung, Rollen und Verwaltung zurück, und F9 wie F10 verlieren ihren Boden. F2 trägt den Werkzeugteil; ohne „ein Artefakt" sind F4 (ein Erzeugnis) und F5 (ein Urteil) nicht durchsetzbar, sondern Vorsätze, und F3 hätte keinen Ort. F6 ist die Bedingung dafür, dass F4 im Betrieb etwas wert ist — ein Artefakt, das seinen Zustand mitschleppt, ist nicht austauschbar. F7 und F8 sind die Sicherheitsschneidung, die dieser Zuschnitt erst ermöglicht: Ohne Kommando-Modus müsste Verwaltung ja irgendwo hin, und der einzige verbleibende Ort wäre die Netzfläche. F9 und F10 hängen zusammen — fällt F9, entsteht der Zwischenzustand, in dem Verwaltungsrechte gebraucht werden, und F10 fällt mit. F11 trägt unabhängig und gilt auch für Dienste, die von diesem Konzept sonst nichts übernehmen.

## 5. Sicherheitsbetrachtung

| Angriff / Risiko | Typischer Hergang | Gegenmaßnahmen |
|---|---|---|
| Übernahme über die Netzfläche | Schwachstelle im Dienstmodus wird ausgenutzt | Der Prozess kann die Konfiguration nicht schreiben, also die Funktion nicht umwidmen (F8); es gibt keine Verwaltungsfunktion, die man erreichen könnte (F7); und keinen Zugang, der mehr trüge als die eine Funktion (F10) |
| Rechteausweitung über einen erbeuteten Zugang | ein gültiges Zugangsartefakt gerät in fremde Hände | Der Zugang trägt genau die vorkonfigurierte Ausprägung der Funktion; Ziel und Umfang stehen nicht in der Anfrage und lassen sich nicht verschieben (F9) |
| Verwaltungsfläche als Einfallstor | Verwaltungspfad oder Debug-Route mit schwacher Anmeldung | Es gibt sie nicht: Verwaltung ist ein lokal gestartetes Kommando, dessen Authentifizierung der Serverzugang ist (F7) |
| Versionsdrift des Verwaltungswerkzeugs | Werkzeug aus älterer Fassung schreibt in die Ablage | Ein Artefakt, eine Version (F2); Kommandos laufen gegen dasselbe Erzeugnis (F4) |
| Verwaltung aus fremder Umgebung | Kommando vom Entwicklerrechner gegen die Produktionsablage | Kommandos nur in derselben Umgebung wie der Dienst (F4); Schreibrechte nur dort (F8) |
| Geheimnis im Artefakt | Konfiguration mit Zugangsdaten ins Image gebaut | Zustand und Geheimnisse liegen außerhalb (F6); die Konfigurationsdatei enthält nur Verweise auf Geheimnisse, nicht die Geheimnisse selbst (F5) |
| Existenz-Orakel | systematisches Durchprobieren fremder Ressourcen | Fremde wie nicht existierende Ressourcen antworten identisch; der Zugang begrenzt den Blick ohnehin auf seine eigene Ausprägung (F9) |
| Veraltete TLS-Bibliothek im Dienst | Dienst terminiert selbst TLS und wird selten aktualisiert | TLS liegt beim Reverse Proxy, der dafür gebaut und gepflegt ist (F11) |
| Unbeabsichtigte Freigabe | Dienst lauscht auf allen Schnittstellen | Lokaler Endpunkt; die Freigabe ist eine bewusste Proxy-Konfiguration (F11) |

Grenze des Modells: Gegen einen Angreifer, der bereits Serverzugang hat, schützt hier nichts mehr — die Kommandos stehen ihm dann so offen wie dem Betreiber, und das ist beabsichtigt: Ihre Authentifizierung *ist* der Serverzugang. Wer diese Annahme nicht treffen kann, findet in §7 die Bedingungen, unter denen der Zuschnitt kippt.

## 6. Was das kostet

Dieser Abschnitt zählt auf, was der Zuschnitt kostet. Jeder Punkt ist an der Festlegung festgemacht, die ihn verursacht, und steht ohne Gegenrechnung.

**Die zweite Funktion kostet einen zweiten Dienst (F1).** Was naheliegt — „das kann der doch gleich mit" —, ist hier verboten, und der Preis ist real: ein zweites Erzeugnis, eine zweite Auslieferung, eine zweite Ablage, ein zweiter Eintrag im Proxy. Wer viele kleine Funktionen hat, verwaltet viele kleine Dienste; die Ersparnis liegt im Inneren jedes einzelnen, nicht in ihrer Zahl.

**Das Artefakt trägt Code, den der Dienst nie ausführt (F2).** Die Kommandos liegen im selben Erzeugnis — sie vergrößern es, sie erweitern die Angriffsfläche des *Images* (wenn auch nicht die des laufenden Prozesses), und sie müssen mitgepflegt, mitgetestet und mitausgeliefert werden.

**Die Auslieferungskette will gepflegt werden (F4).** Bauleitung, Registry, Zugangsdaten der Zielmaschine, und bei nicht öffentlicher Quelle die Frage, wer das Image überhaupt ziehen darf: Für ein Nachmittagsprojekt ist das der Teil, der leicht größer wirkt als der Dienst selbst. Wer darauf verzichtet und die Zielmaschine bauen lässt, spart diese Arbeit — und bezahlt mit der Vervielfachung des Artefakts, die F4 gerade verhindern soll.

**Kommandozeilen-Ergonomie muss gebaut werden (F2, F3).** Eine Verwaltung, die niemand bedienen mag, wird umgangen — mit Skripten, die direkt in die Ablage schreiben. Also braucht es sprechende Kommandos, brauchbare Ausgaben, verlässliche Exit-Codes, eine maschinenlesbare Form und eine Hilfe, die man ohne Handbuch versteht. Das ist Arbeit, die in einer Weboberfläche sichtbarer wäre und deshalb oft eher gemacht wird.

**Betriebsaufgaben brauchen Serverzugang (F7).** Konfiguration prüfen, Zugänge ausstellen, Bestand aufräumen: Dafür gibt es keine Oberfläche, sondern die Kommandozeile auf dem Server. Für den kleinen Dienst, dessen Erbauer und Betreiber dieselbe Person sind, ist das eine Ersparnis und kein Verlust. Wächst der Betrieb und sollen Bereitschaft oder Support solche Aufgaben übernehmen, brauchen sie Systemzugang — oder es braucht doch eine Verwaltungsfläche mit allem, was daran hängt. Das fachliche Geschehen ist davon unberührt: Was der Dienst als seine Funktion anbietet, bleibt über seine Netzfläche erreichbar.

**Jede Änderung der Ausprägung ist ein neuer Zugang (F9).** Soll dieselbe Anwendung künftig in einen anderen Ordner schreiben, wird nicht ein Feld geändert, sondern ein Zugang neu ausgestellt und der alte entzogen — mit allem, was das beim Aufrufer bedeutet. Die Steifheit ist gewollt; bequem ist sie nicht.

**Der Rechte-Schnitt kostet Sorgfalt im Deployment (F8).** Getrennte Benutzer, Dateirechte je Verzeichnis, ein Kommandoaufruf unter dem richtigen Benutzer — Konfigurationsarbeit in der Prozessform, fehleranfällig und in jeder Umgebung erneut zu leisten.

**Der Reverse Proxy ist eine zusätzliche Komponente (F11).** Er muss ausgewählt, konfiguriert, aktualisiert und im Störfall mitgedacht werden; die Fehlersuche bekommt eine Station mehr.

**Zustand außerhalb heißt Zustand verwalten (F6).** Benannte Orte, Rechte, Sicherungen, Wiederherstellung: Was das Artefakt nicht mitbringt, muss der Betrieb bereitstellen. Der Preis der Austauschbarkeit ist, dass es etwas gibt, das nicht ausgetauscht wird.

## 7. Geltung und Grenzen

### Randbedingungen

**Die Aufgabe lässt sich als eine Funktion fassen.** Es muss einen Satz geben, der sagt, was der Dienst tut, ohne „und außerdem" (F1). Man erkennt die Lage beim Versuch, ihn zu schreiben.

**Wer den Dienst verwaltet, hat Serverzugang.** Die Kommandos verzichten auf eigene Anmeldung, weil sie ihn voraussetzen (F7). Gemeint sind Betriebsaufgaben am Dienst, nicht seine fachliche Benutzung. Man erkennt die Lage daran, wer Zugänge ausstellen und Konfiguration ändern soll — sind das andere als die Systemverantwortlichen, kippt die Annahme (unten).

**Die Prozessform erlaubt Einmal-Aufrufe im selben Umfeld.** Ob systemd, Container-Laufzeit oder schlichtes Binary: Es muss möglich sein, dasselbe Erzeugnis kurz und lokal auszuführen (F4).

**Zustand lässt sich außerhalb ablegen.** Benannte Verzeichnisse, ein Datenträger, ein externer Speicher — irgendetwas, das den Fassungswechsel überlebt (F6).

### Wann die Antwort kippt

**Der Dienst muss mehrere unabhängige Funktionen tragen.** Wenn zwei Dinge zwingend denselben Zustand teilen und sich nicht auf zwei Dienste schneiden lassen, kehrt die Rechteunterscheidung zurück: Dann braucht es Zugänge unterschiedlicher Art, eine Regel, wer welche bekommt — und damit die Verwaltung, die F9 und F10 gerade abschaffen. Man erkennt die Lage daran, dass eine Ausprägung des Zugangs die eine Methode erlauben soll und die andere nicht.

**Betriebsaufgaben sollen ohne Systemzugang erledigt werden.** Gemeint sind Aufgaben am Dienst selbst — Zugänge ausstellen, Konfiguration ändern, Bestand aufräumen —, die Menschen ohne Serverzugang übernehmen sollen: Bereitschaft, Support, Fachadministration. Die Antwort kippt, weil F7 seine Einfachheit genau daraus bezieht, dass der Serverzugang die Authentifizierung ist; man müsste sonst vielen Menschen Systemrechte geben, um ihnen Betriebsaufgaben zu geben. Dann braucht es doch eine authentifizierte Verwaltungsfläche — und mit ihr die Anmelde-, Rechte- und Nachweisfragen, die F7 vermeidet. Man erkennt die Lage an der Frage „Kann das jemand ohne Serverzugang machen?", und daran, dass sie regelmäßig gestellt wird. Nicht gemeint ist die fachliche Nutzung: Dass Anwender die Funktion über das Netz benutzen, ist der Zweck und keine Kipp-Bedingung.

**Die Plattform gibt keinen Einmal-Aufruf her.** Manche Betriebsumgebungen erlauben nur das Ausrollen langlaufender Prozesse, keinen Zugriff auf eine laufende Instanz. Dann lässt sich F4 nicht einlösen; die Kommandos müssten als eigener, kurzlebiger Prozess desselben Images ausgerollt werden — machbar, aber der Zuschnitt wird umständlich, und man sollte das vorher wissen.

**Der Dienst besteht aus mehreren Laufzeiten.** Sind Teile in verschiedenen Sprachen gebaut, gibt es kein „ein Binary" mehr. Dann gilt das Prinzip nur noch je Komponente, und die Frage nach dem gemeinsamen Verwaltungsweg stellt sich neu (Q2).

**Nachweispflicht je Person.** Wo belegt werden muss, wer wann welche Betriebshandlung vorgenommen hat, reicht „hatte Serverzugang" nicht; die Prozessform muss das leisten, oder die Verwaltungsfläche kehrt zurück.

### Was es nicht löst

**Skalierung und Ausfallsicherheit.** Ob ein Dienst in mehreren Instanzen läuft, wie Lasten verteilt und Übernahmen geregelt werden, sagt dieses Konzept nicht; es macht das Artefakt nur austauschbar genug, dass solche Verfahren möglich bleiben.

**Datenwanderungen.** Wann und wie Schemaänderungen ausgeführt werden — beim Start oder als Kommando —, ist offen und wird kontrovers gehandhabt (Q4).

**Beobachtbarkeit und Sicherung.** Protokolle, Kennzahlen, Alarme, Datensicherung und Wiederherstellung bleiben Betriebsarbeit außerhalb dieses Zuschnitts.

**Das fachliche Benutzer- und Rollenmodell.** Benutzer, Rollen und Freigaben *innerhalb* der Funktion eines Dienstes stehen quer zu diesem Zuschnitt: Er verbietet sie nicht, schreibt sie nicht vor und ersetzt sie nicht. Zugänge sind hier der eine definierte Weg von außen; wie ein Dienst intern modelliert, bleibt seine Sache. Dieses Dossier sagt allein, dass die *Verwaltung des Dienstes* kein solches Modell braucht (F10).

## 8. Bewährung

Dieser Abschnitt zeigt, was gebaut wurde, und trennt es von dem, was noch aussteht. Anders als in den Schwester-Dossiers ist das durchgehende Beispiel hier **nicht erfunden**: Der Dienst ist genau nach diesem Zuschnitt spezifiziert, deshalb dient er zugleich als Beleg und als Durchlauf von Anfang bis Ende. Alle genannten Werte stammen aus seiner Kurzspezifikation. Belegt ist damit der Bauzuschnitt; Betriebserfahrung ist es noch nicht. **Und eine Warnung vorweg:** Die konkreten Namen und Werte hier — Kommandobezeichnungen, Token-Präfix, Konfigurationsformat, der Arbeiter-Strang für die Warteschlange — sind *Illustration dieses einen Dienstes*, keine Hauskonvention. Verbindlich sind das Vorgehen aus §2 und die Festlegungen aus §4; wer diesen Abschnitt abschreibt, statt ihn zu lesen, baut Apparate nach, die seine Funktion womöglich gar nicht braucht.

### Der Entwurfsablage-Dienst, von Anfang bis Ende

**Eine Funktion (F1).** Der Dienst stellt eine Mail in einen IMAP-Ordner ein — das ist alles. Die Hauptmethode nimmt die Nachricht entgegen (`POST /messages`, Antwort `202` mit einer Vorgangs-Kennung); die Hilfsmethoden sind der Datei-Upload für Anhänge und HTML-Körper (`POST /files`, danach `GET`/`DELETE` auf die Datei), die Statusabfrage zum Vorgang (`GET /messages/{id}`) und eine Bereitschaftsanzeige (`GET /healthz`). Alle teilen denselben Zugang. Bemerkenswert ist auch hier, was der Dienst *nicht* tut: Er hält nichts vor, durchsucht nichts, bietet keinen Abruf abgelegter Mails an — Aufbewahrung und Auffindbarkeit leistet das Postfach, und jeder Mail-Client kann darauf zugreifen. Ein eigener Speicher daneben wäre die zweite Quelle mit allen Folgen.

**Ein Artefakt mit Modi (F2).** Es gibt genau ein Programm. `serve` startet den Dienst; `check` prüft die Konfiguration und kann mit `--live` zusätzlich die Verbindung zum Zielsystem testen; `status` zeigt lokale Kennzahlen und Warteschlangenstände; unter `config` liegen Einrichten, Auflisten und Ändern von Sites und Konten samt `rotate-token`; `queue` und `files` erlauben den ausdrücklichen Eingriff in Warteschlange und Dateipool; dazu `version`. Der Arbeiter-Strang für die Zustellung gehört zum selben Programm.

**Skriptbare Kommandos (F3).** Die Exit-Codes sind festgelegt: 0 alles gut · 1 Laufzeitfehler · 2 falscher Aufruf · 3 Konfiguration ungültig · 4 Live-Test fehlgeschlagen. Auflistende Kommandos und `status` bieten `--json`.

**Ein Erzeugnis (F4).** Ausgeliefert werden ein Python-Paket und ein Container-Image (`ENTRYPOINT ["imap-service","serve"]`, non-root, Bereitschaftsprüfung auf den Health-Endpunkt) samt einem `deploy/`-Verzeichnis mit Dockerfile, Compose-Datei, Proxy- und systemd-Konfiguration. Kommandos laufen gegen dasselbe Image.

**Eine Konfiguration, ein Urteil (F5).** Die Konfiguration ist eine TOML-Datei; gesucht wird sie in der Reihenfolge `--config-file` → `$IMAP_SERVICE_CONFIG` → benutzernaher Ort → `/etc/imap-service/config.toml`, und die Kommandos zeigen an, welche Datei sie verwenden. Gelesen wird mit der Standardbibliothek, geschrieben formaterhaltend, validiert mit feldgenauen Fehlermeldungen. Postfach-Passwörter stehen nicht in der Datei, sondern als Verweis (`env:VAR` oder `file:/pfad`).

**Zustand daneben (F6).** Ein Datenverzeichnis je Site und Zugang, mit getrennten Bereichen für hochgeladene Dateien und Warteschlangenstände. Ein Fassungswechsel tauscht das Image; die Ablage bleibt.

**Keine Verwaltungsfläche (F7).** Die Netzfläche kennt keinen Verwaltungspfad. Der einzige unauthentifizierte Punkt ist `GET /healthz`, und er antwortet ausschließlich mit `{"status":"ok"}` beziehungsweise 503 — keine Kennzahlen, keine Interna. Damit ist er genau der Betriebspunkt, den F7 zulässt, und ein brauchbarer Prüfstein für die offene Frage Q3.

**Der Rechte-Schnitt (F8).** Der Dienst läuft als eigener, nicht privilegierter Benutzer; die systemd-Unit setzt `ProtectSystem=strict` und weist Konfigurations- und Zustandsverzeichnis getrennt zu. An der Konfiguration geschrieben wird nur im Kommando-Modus.

**Konfiguration und Zugang in einem Akt (F9).** `config add-account` ist der Kern: Es konfiguriert die Funktion für diesen Anwendungsfall — Site, IMAP-Konto, Ziel-Ordner — und erzeugt dafür das Token, in einem Handgriff. Das Token verkörpert genau dieses Tripel; Aufrufer wählen niemals Konto oder Ordner, beides steht nicht in ihrer Anfrage. Ausgegeben wird es genau einmal (`imsvc_` plus 24 zufällige Zeichen), persistiert werden nur sein SHA-256 und eine harmlose Kurz-Kennung (`tk_` plus sechs Zeichen). `rotate-token` ersetzt es atomar.

**Keine Verwaltungsrechte (F10).** Es gibt kein Verwaltungs-Token und keinen Verwaltungsbenutzer im Dienst; die drei Rollen der Spezifikation heißen Betreiber, Client/Agent und Anwender — ein Admin kommt nicht vor. Jeder Zugriff ist auf *(Site, Zugang)* gefiltert, und fremde wie nicht existierende Ressourcen antworten identisch mit 404.

**Außenkante (F11).** Der Dienst spricht ausschließlich HTTP und lauscht standardmäßig auf `127.0.0.1:8025`; TLS terminiert der vorgelagerte Proxy, der als einziger veröffentlichter Einstieg dient. Als Prozessform sind Container-Betrieb und gehärtete systemd-Unit vorgesehen — dasselbe Artefakt in beiden Fällen.

**Stand, ehrlich.** Die Spezifikation ist vollständig, die Implementierung durch einen Entwicklungs-Agenten läuft; eine Vollzugsmeldung stand bei Niederschrift dieser Fassung noch aus und wird hier nachgetragen. Belegt ist damit, dass sich dieser Zuschnitt für einen realen Dienst vollständig und widerspruchsfrei ausspezifizieren lässt — nicht, wie er sich über Jahre im Betrieb hält. Insbesondere die Kosten aus §6, die erst mit der Zeit anfallen — Pflege des Kommando-Codes, Ergonomie, die Steifheit aus F9 —, sind noch nicht eingetreten und deshalb auch nicht widerlegt.

### Herkunft

Der Zuschnitt ist keine Erfindung dieses Dossiers. Die Zwölf-Faktoren-Sammlung fordert seit Langem, Verwaltungsprozesse in identischer Umgebung wie den Dienst laufen zu lassen und ihren Code mit dem Anwendungscode auszuliefern (Quelle 2). Dieses Dossier zieht daraus die schärfere Konsequenz — nicht „mitgeliefert", sondern „dasselbe Artefakt in einem anderen Modus" — und verbindet sie mit zwei Dingen, die dort nicht vorkommen: der einen Funktion (F1) und der Erzeugung des Zugangs im Konfigurationsschritt (F9), die zusammen das Rechtethema auflösen, bevor es entsteht.

## 9. Offene Fragen

Die ersten beiden Fragen sind zugleich **Aufträge an den Verwender**: Ihre richtige Antwort hängt vom Einsatz ab; wer den Zuschnitt übernimmt, beantwortet sie für seinen Kontext und dokumentiert die Antwort.

**Q1** *(Auftrag)* Welche Rechte braucht jeder Modus genau (F8) — welcher Benutzer, welche Verzeichnisse, lesend oder schreibend? Der Schnitt ist dienstabhängig; das Konzept verlangt nur, dass er existiert, in der Prozessform durchgesetzt und dokumentiert ist.

**Q2** *(Auftrag)* Wie weit reicht „ein Artefakt", wenn der Dienst aus mehreren Komponenten oder Laufzeiten besteht? Zu klären ist, ob das Prinzip je Komponente gilt — und wer dann die Verwaltung führt, die über Komponentengrenzen hinweg nötig ist.

**Q3** Wo verläuft die Grenze zwischen Betriebspunkt und Verwaltungsfläche (F7)? Eine Bereitschaftsanzeige, die nur „läuft" sagt, ändert nichts und verrät nichts; eine Kennzahlen-Ausgabe verrät bereits Betriebsinterna; eine Konfigurationsanzeige wäre eindeutig Verwaltung. Braucht es dafür eine Regel, oder genügt der Grundsatz „nichts, was ändert, und nichts, was Inhalte oder Interna zeigt"?

**Q4** Gehören Datenwanderungen in ein Kommando (ausdrücklich aufgerufen) oder in den Start des Dienstmodus (automatisch)? Der Kommando-Weg macht sie zur Entscheidung und passt zu F8; der Start-Weg macht sie unvergesslich und passt zu automatisierten Ausrollverfahren. Beides ist verbreitet, und die Wahl hat Folgen für Rollbacks.

**Q5** Wie prüft man, dass Prüfkommando und Startverhalten wirklich dasselbe Urteil fällen (F5) — durch geteilten Code allein, oder braucht es einen Test, der beide Wege gegeneinander hält?

**Q6** Wie verhält sich dieser Zuschnitt zu Betriebsumgebungen, die ihre eigene Verwaltungsebene mitbringen (Orchestrierer mit deklarativer Konfiguration)? Wird der Kommando-Modus dort überflüssig, verlagert er sich — oder bleibt er der Ort für alles, was die Plattform nicht kennt?

**Q7** Wann trägt ein Dienst legitim mehr als eine Funktion (F1)? Der klare Fall ist gemeinsamer Zustand, der sich nicht schneiden lässt. Braucht es dafür ein Kriterium — etwa „zwei Funktionen sind eine, wenn keine ohne die andere sinnvoll ist" —, oder bleibt es eine Ermessensfrage, die man am Beschreibungssatz prüft?

## 10. Quellen & Verwandtes

Dies ist der einzige Ort dieses Dossiers, an dem Verweise stehen. Fundstellen geprüft am 15.08.2026.

**Quelle 1 · Schwester-Konzept dieser Reihe: [capability-tokens-for-agents](../capability-tokens-for-agents/PROPOSAL.de.md).** — *Wofür sie hier steht:* Die Begründung des Zugangs-Musters, das F9 voraussetzt (ein Token, eine Fähigkeit, ein Ort; serverseitige Erzeugung, Einmal-Anzeige, Hash-Speicherung, einzelner Entzug) und der Geheimnis-Rangfolge, auf die F5 verweist; dort auch die Selbstregistrierung, die F9 für diesen Zuschnitt begründet nicht übernimmt. Vertiefung, kein Verständnis-Muss.

**Quelle 2 · Adam Wiggins u. a.: *The Twelve-Factor App*, Faktor XII „Admin processes".** https://12factor.net/admin-processes — *Wofür sie hier steht:* Die Ahnenreihe des Prinzips: Verwaltungsprozesse sollen „in an identical environment as the regular long-running processes of the app" laufen, und „admin code must ship with application code to avoid synchronization issues". F2 und F4 verschärfen das zu einem gemeinsamen Artefakt; F1, F9 und F10 gehen darüber hinaus. Die Sammlung empfiehlt außerdem Konfiguration in Umgebungsvariablen — davon weicht F5 für alles außer Geheimnisse bewusst ab, weil eine Datei als Ganzes prüfbar und versionierbar ist.

**Quelle 3 · Entwurfsablage-Dienst des Autors, Repository `imap-service`.** https://github.com/schlingensiepen/imap-service — *Wofür sie hier steht:* Das durchgehende Beispiel und der Beleg in §8; alle dort genannten Werte stammen aus seiner Kurzspezifikation.

**Quelle 4 · Schwester-Konzept dieser Reihe: [self-describing-services](../self-describing-services/PROPOSAL.de.md).** — *Wofür sie hier steht:* Dieselbe Denkfigur auf der Beschreibungsebene — eine Quelle statt verteilter Kopien —, wo dieses Dossier sie auf Werkzeug und Zuschnitt anwendet. Vertiefung, kein Verständnis-Muss.

**Einordnung.** Dieses Dossier und `self-describing-services` (Quelle 4) sind bewusst getrennt und nicht zwei Fassungen derselben Idee: Dort steht ein **Entwurfsmuster für eine Schnittstelle** — wie ein Dienst sich selbst beschreibt, damit Konsumenten ihn ohne Installation benutzen können. Hier steht ein **Vorgehen**, wie man einen solchen Dienst zügig und ohne Wildwuchs zum Laufen bringt. Sie berühren sich an genau zwei Stellen: Der nach diesem Vorgehen gebaute Dienst kann sein Beschreibungsmuster von dort übernehmen, und der Zugang, den Schritt 8 ausstellt, folgt dem Muster aus Quelle 1. Man kann jedes der drei Dossiers ohne die beiden anderen anwenden.
