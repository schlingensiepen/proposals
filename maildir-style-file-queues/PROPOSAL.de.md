---
type: Konzept
status: Entwurf
version: "0.9 · Materialfassung zur Fertigstellung"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: maildir-style-file-queues
language: de (führend)
question: "Wie übergebe ich Arbeitsaufträge zwischen nebenläufigen Teilen eines kleinen Dienstes, ohne eine Datenbank und ohne Sperren?"
thesis: "Ein Verzeichnisbaum ist die Warteschlange: Jeder Auftrag ist eine Datei, sein Zustand ist der Ordner, in dem sie liegt, und jeder Zustandswechsel ist ein atomares Umbenennen auf demselben Dateisystem — das ist lockfrei, absturzfest, mit den Bordmitteln des Betriebssystems inspizierbar und reparierbar."
---

<!-- MATERIALFASSUNG. Dieses Dossier enthält vollständig, was in der Entwurfsarbeit am
     Entwurfsablage-Dienst zu diesem Thema entschieden und erwogen wurde, einschließlich
     der verworfenen Wege. Es ist inhaltlich vollständig, aber noch nicht durch die
     Endredaktion der Reihe gegangen: Die englische Fassung, AGENT.md, README und
     CHANGELOG fehlen; §8 braucht die Vollzugsmeldung der Implementierung. Wer es
     fertigstellt, findet die Konventionen in AGENTS.md und agents/create-or-maintain.md
     des Repositoriums. F- und Q-Nummern sind ab jetzt stabil zu halten. -->

# Der Verzeichnisbaum als Warteschlange

**In einem Satz:** Wer in einem kleinen Dienst Arbeitsaufträge von der Annahme zur Ausführung übergeben muss, legt jeden Auftrag als Datei ab und macht den Ordner, in dem sie liegt, zu ihrem Zustand — Zustandswechsel sind atomare Umbenennungen, und damit braucht es weder Datenbank noch Sperren noch eigene Wiederanlauf-Logik.

**Kurzthese:** Sobald ein Dienst etwas annimmt, das später ausgeführt wird, steht die Frage nach der Warteschlange im Raum, und der Reflex heißt Datenbank. Für einen kleinen Dienst ist das die teurere Antwort: Sie bringt Nebenläufigkeitsfragen, ein Schema, eine Migrationsgeschichte und ein Werkzeug mit, das man zum Nachsehen erst starten muss. Das Maildir-Muster — Datei je Auftrag, Ordner als Zustand, atomares Umbenennen als Übergang — leistet dasselbe mit Bordmitteln, ist mit `ls` einsehbar und mit `mv` reparierbar.

## 1. Problem

Ein Dienst nimmt etwas entgegen, das er nicht sofort erledigen kann oder soll: eine Nachricht, die zugestellt werden muss, eine Datei, die verarbeitet wird, einen Auftrag an ein fremdes System, das gerade nicht antwortet. Zwischen der Annahme und der Ausführung liegt eine Warteschlange. Der Autor stand vor genau diesem Fall: Ein Dienst nimmt Entwürfe entgegen und legt sie per IMAP in Postfächern ab; die Annahme antwortet sofort, die Ablage dauert, und der Zielserver ist gelegentlich nicht erreichbar.

Damit stehen sofort die üblichen Fragen im Raum. Mehrere Annahme-Fäden reihen gleichzeitig ein — wie verhindert man, dass sie sich in die Quere kommen? Was passiert, wenn der Dienst mitten in der Verarbeitung stirbt? Woran erkennt man, was gerade wartet und was schiefging? Und wie beantwortet man einem Aufrufer die Frage nach dem Stand seines Auftrags?

Der Reflex lautet: eine Datenbank. Sie beantwortet alle vier Fragen, und sie bringt vier neue mit — ein Schema, das mitwandern muss; eine Nebenläufigkeitssemantik, die man verstehen muss; eine Abhängigkeit, die im Auslieferungserzeugnis mitreist; und ein Werkzeug, das man erst starten muss, um im Störfall nachzusehen. Für ein System mit Millionen Aufträgen pro Stunde ist das der richtige Preis. Für einen kleinen Dienst ist es der Preis dafür, ein gelöstes Problem noch einmal zu lösen.

Drei Rollen tragen das Problem. **Der Erbauer** muss die Übergabe korrekt bekommen — nebenläufig, absturzfest, ohne Doppelverarbeitung. **Der Betreiber** muss im Störfall sehen können, was anliegt, und eingreifen können, ohne den Dienst anzuhalten. **Der Aufrufer** will wissen, was aus seinem Auftrag geworden ist.

## 2. Das Konzept

Der Kern ist eine Umdeutung: **Der Ordner ist der Zustand.** Jeder Auftrag ist eine Datei. In welchem Verzeichnis sie liegt, sagt, wo sie im Ablauf steht — angenommen, wartend, in Arbeit, erledigt, gescheitert. Ein Zustandswechsel ist keine Schreiboperation auf einem Datensatz, sondern ein Umbenennen der Datei von einem Verzeichnis ins nächste.

Das trägt, weil `rename()` innerhalb eines Dateisystems **atomar** ist: Die Datei ist entweder am alten Ort oder am neuen, nie dazwischen und nie an beiden. Daraus folgt alles Weitere von selbst. Das Einreihen braucht keine Sperre, weil jeder Faden nur seine eigene Datei schreibt — erst vollständig in einen Übergabeordner, dann ein Umbenennen in die Warteschlange; halbfertige Aufträge werden nie sichtbar. Das Übernehmen durch einen Arbeiter braucht keine Sperre, weil das Umbenennen selbst der Anspruch ist: Wem es gelingt, der hat den Auftrag; wem es fehlschlägt, weil die Datei nicht mehr da ist, der war zu langsam und geht weiter. Und der Wiederanlauf nach einem Absturz ist eine Aufräumregel von zwei Zeilen: Was in Arbeit lag, wandert zurück in die Warteschlange, und die Übergabeordner werden geleert.

Das Muster ist alt und bewährt: Es ist das **Maildir-Prinzip** aus der Mailserver-Welt (`tmp`/`new`/`cur`), entstanden, um genau dieses Problem ohne Sperren über NFS-Grenzen hinweg zu lösen. Neu ist hier nur die Übertragung auf die Auftragswarteschlange eines kleinen Dienstes — und die Einsicht, dass der zweite große Vorteil nicht die Technik ist, sondern die **Bedienbarkeit im Störfall**: Der Zustand der Warteschlange ist mit `ls` sichtbar und mit `mv` korrigierbar, von jedem, der eine Kommandozeile bedienen kann, ohne Zusatzwerkzeug und ohne den Dienst anzuhalten.

Der Preis dafür ist ebenso klar: Alles, was in einer Datenbank eine Abfrage wäre, ist hier ein Verzeichnisdurchlauf. Übersichten, Zählungen, Suchen über Merkmale — alles kostet lineare Zeit über die Anzahl der Aufträge. Das Muster hat damit eine Größenordnung, jenseits derer es kippt (§7).

## 3. Begriffe

**Auftrag** — die kleinste Arbeitseinheit, die die Warteschlange transportiert; hier: genau eine Datei.

**Zustandsordner** — ein Verzeichnis, dessen Name den Zustand der darin liegenden Aufträge bezeichnet.

**Übergabeordner** — das Verzeichnis, in dem eine Auftragsdatei entsteht, bevor sie vollständig ist; ihr Inhalt ist nie Teil des Ablaufs. In der Mailserver-Tradition heißt er `tmp`.

**Atomares Umbenennen** — die Betriebssystem-Zusage, dass eine Datei bei `rename()` innerhalb eines Dateisystems entweder vollständig am Zielort erscheint oder gar nicht; es gibt keinen Zwischenzustand und keine halb sichtbare Datei.

**Anspruch (Claim)** — die Übernahme eines Auftrags durch einen Arbeiter, hier vollzogen durch das Umbenennen in den Bearbeitungsordner; das Gelingen des Umbenennens *ist* der Anspruch.

**Wiederanlauf-Regel** — die Aufräumoperation beim Start, die einen Absturz mitten in der Verarbeitung heilt.

**Journal** — die Zustandsordner für Erledigtes und Gescheitertes, die nicht automatisch geleert werden und damit zur Historie werden.

**Idempotenz-Marker** — eine leere oder minimale Datei, deren exklusives Anlegen (`O_EXCL`) die Frage „habe ich diesen Auftrag schon einmal angenommen?" ohne weiteren Speicher beantwortet.

## 4. Festlegungen

**Landkarte.** Neun Festlegungen. **F1–F3** tragen das Muster selbst: Datei je Auftrag, Ordner als Zustand, atomares Umbenennen als einziger Übergang. **F4–F6** regeln die Nebenläufigkeit: das Einreihen, das Übernehmen, die Wiederholung. **F7** ist der Wiederanlauf. **F8** und **F9** betreffen den Betrieb: die Nicht-Löschung von Nutzdaten und die Sichtbarkeit. F3 trägt alles — fällt die Atomizität des Umbenennens (etwa auf einem Netzdateisystem), zerfällt das ganze Muster zu einer Sammlung von Wettläufen.

### F1 · Ein Auftrag, eine Datei

**Gilt für:** Erbauer.

**Was gilt.** Jeder Auftrag liegt vollständig in genau einer Datei; sie enthält alles, was zur Ausführung nötig ist, in bereits geprüfter Form. Die Annahmestelle prüft und übersetzt die Anfrage einmal und schreibt das Ergebnis; der Arbeiter liest die Datei und muss nicht erneut prüfen, was schon geprüft ist. Der Dateiname trägt eine kryptografisch feste Kennung und eine grobe Ordnungsangabe (etwa einen Zeitstempel voran, für ungefähre Reihenfolge).

**Warum.** Ein Auftrag, der in mehreren Dateien liegt, hat einen Zwischenzustand, in dem die eine Datei schon da ist und die andere nicht — und damit fängt die ganze Fragestellung wieder von vorn an, die dieses Muster löst. Und weil die Datei bereits Geprüftes enthält, kann der Arbeiter niemals an einer Prüfung scheitern, die die Annahmestelle schon bestanden hat: Wer angenommen wurde, bekommt kein „ungültig" mehr, nur noch „konnte nicht zugestellt werden".

**Naheliegend und verworfen.** *Die Rohanfrage speichern und erst im Arbeiter prüfen* — verschiebt Fehler in einen Kontext ohne Aufrufer, dem man sie nicht mehr melden kann. *Mehrere Dateien je Auftrag (Kopf, Nutzlast, Anhänge)* — jede zusätzliche Datei ist ein weiterer Wettlauf; Anhänge gehören in einen getrennten, dauerhaften Pool, auf den der Auftrag verweist (F8). *Fortlaufende Nummern als Kennung* — verraten Durchsatz und erlauben das Erraten fremder Kennungen; eine Zufallskennung tut das nicht.

**Woran man merkt, dass es verletzt ist.** Ein Auftrag scheitert im Arbeiter an etwas, das die Annahmestelle hätte sehen können — oder es gibt Aufträge, die „halb da" sind.

### F2 · Der Ordner ist der Zustand

**Gilt für:** Erbauer.

**Was gilt.** Der Ablauf hat eine feste, kleine Menge von Zuständen, und jeder ist ein Verzeichnis. Der Zustand eines Auftrags wird nirgends sonst geführt — nicht in der Datei, nicht in einem Index, nicht im Arbeitsspeicher. Wer den Zustand wissen will, sieht nach, wo die Datei liegt. Bewährt hat sich: Übergabe · Wartend · In Arbeit · Erledigt · Gescheitert.

**Warum.** Zwei Orte für dieselbe Wahrheit driften; ein Zustandsfeld *in* der Datei müsste zusammen mit der Bewegung der Datei geändert werden, und genau diese Gleichzeitigkeit gibt es nicht. Der Ordner ist zudem der einzige Ort, den das Betriebssystem ohnehin atomar verwaltet — man bekommt die Konsistenz geschenkt, statt sie zu bauen.

**Naheliegend und verworfen.** *Zustandsfeld in der Auftragsdatei, alle Aufträge in einem Verzeichnis* — verlangt Sperren beim Ändern und macht jede Übersicht zum Lesen aller Dateien. *Ein zusätzlicher Index für schnelle Abfragen* — die zweite Wahrheit, die genau dann falsch ist, wenn man sie braucht; wer Abfragen braucht, hat die Größenordnung dieses Musters verlassen (§7).

**Woran man merkt, dass es verletzt ist.** Es gibt eine Stelle im Code, die den Zustand eines Auftrags anders bestimmt als über sein Verzeichnis.

### F3 · Jeder Übergang ist ein atomares Umbenennen

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Zustandswechsel geschehen ausschließlich durch Umbenennen der Datei von einem Zustandsordner in den nächsten. Auch Änderungen *am Inhalt* einer Auftragsdatei geschehen so: neue Fassung im Übergabeordner schreiben, dann über die alte Datei umbenennen. Betriebsvoraussetzung ist, dass **alle** Zustandsordner auf demselben lokalen Dateisystem liegen — Netzdateisysteme sind ausgeschlossen, weil die Atomizität dort nicht zugesichert ist.

**Warum.** `rename()` ist die einzige verbreitete Dateisystem-Operation mit einer harten Atomizitätszusage. Auf ihr ruht alles: das lockfreie Einreihen, der Anspruch des Arbeiters, die Absturzfestigkeit. Wer stattdessen kopiert und löscht oder eine Datei an Ort und Stelle überschreibt, hat sofort einen Zwischenzustand — und damit alle Wettläufe zurück, die das Muster vermeidet.

**Naheliegend und verworfen.** *Kopieren und dann löschen* — erzeugt einen Moment, in dem der Auftrag zweimal existiert; ein Absturz dazwischen führt zur Doppelverarbeitung. *Datei an Ort und Stelle überschreiben* — ein Leser kann eine halbe Datei sehen. *Sperrdateien statt Umbenennen* — bringt die Frage zurück, wer eine verwaiste Sperre wann aufräumt. *Verteilung über ein Netzdateisystem* — die Atomizitätszusage gilt dort nicht durchgängig; wer es dennoch tut, baut auf Sand.

**Woran man merkt, dass es verletzt ist.** Ein Auftrag wird zweimal ausgeführt — oder ein Leser stolpert über eine unvollständige Datei.

### F4 · Einreihen ohne Sperre

**Gilt für:** Erbauer.

**Was gilt.** Die Annahmestelle schreibt die vollständige Auftragsdatei in den Übergabeordner, erzwingt ihr Ausschreiben auf den Datenträger und benennt sie dann in den Wartend-Ordner um. Mehrere Annahme-Fäden arbeiten dabei ohne jede Abstimmung, weil jeder ausschließlich seine eigene Datei anfasst.

**Warum.** Sperren sind der teuerste Teil jeder Warteschlangen-Umsetzung — sie müssen erworben, freigegeben und im Absturzfall aufgeräumt werden. Hier entfallen sie ersatzlos, weil es keinen gemeinsam beschriebenen Speicher gibt: Die einzige Interaktion zwischen den Fäden ist, dass ihre fertigen Dateien im selben Verzeichnis landen. Das Ausschreiben vor dem Umbenennen ist nötig, damit ein Stromausfall keine leere, aber vorhandene Auftragsdatei hinterlässt.

**Naheliegend und verworfen.** *Direkt in den Wartend-Ordner schreiben* — dann kann der Arbeiter eine halb geschriebene Datei greifen. *Auf das Ausschreiben verzichten („geht schon")* — geht meistens; der Ausnahmefall ist ein Auftrag, der bestätigt wurde und leer ist.

**Woran man merkt, dass es verletzt ist.** Im Wartend-Ordner liegen unvollständige oder leere Dateien.

### F5 · Der Anspruch ist das Umbenennen

**Gilt für:** Erbauer.

**Was gilt.** Ein Arbeiter übernimmt einen Auftrag, indem er ihn in den In-Arbeit-Ordner umbenennt. Gelingt das Umbenennen, gehört ihm der Auftrag; schlägt es fehl, weil die Datei nicht mehr existiert, hat ein anderer sie zuerst bekommen — dann wird das Fehlschlagen stillschweigend hingenommen und der nächste Auftrag genommen. Es gibt keine weitere Abstimmung zwischen Arbeitern.

**Warum.** Das ist derselbe Trick wie beim Einreihen, nur andersherum: Die Atomizität des Umbenennens ist bereits ein Ausschlussverfahren, also braucht man keines zu bauen. Der Ansatz skaliert außerdem unverändert von einem Arbeiter auf mehrere, ohne dass sich am Code etwas ändert.

**Naheliegend und verworfen.** *Zuerst lesen, dann als übernommen markieren* — zwischen Lesen und Markieren liegt das Zeitfenster für die Doppelübernahme. *Ein Arbeiter, der als einziger die Warteschlange kennt* — funktioniert, verhindert aber jede spätere Parallelisierung und macht den Arbeiter zum Engpass. *Eine Koordinationsschicht (Sperrdienst, Datenbanktransaktion)* — löst ein Problem, das man an dieser Stelle gar nicht hat.

**Woran man merkt, dass es verletzt ist.** Zwei Arbeiter bearbeiten denselben Auftrag — oder ein Arbeiter behandelt das Fehlschlagen der Übernahme als Fehler und meldet es.

### F6 · Wiederholung bleibt in der Warteschlange

**Gilt für:** Erbauer.

**Was gilt.** Ein Auftrag, der scheitert und wiederholt werden soll, wandert zurück in den Wartend-Ordner; der Zeitpunkt des nächsten Versuchs und die Zahl der bisherigen Versuche stehen *in der Datei*. Der Arbeiter überspringt Aufträge, deren Zeitpunkt noch nicht erreicht ist. Erst wenn nicht mehr wiederholt wird, wandert der Auftrag in den Gescheitert-Ordner, mit dem letzten Fehler in der Datei.

**Warum.** Ein eigener Wiederholungs-Ordner wäre ein weiterer Zustand mit eigener Aufräumregel, ohne dass sich etwas gewinnen ließe. Und die Wiederholungsdaten gehören in die Datei und nicht in den Dateinamen, weil sie sich ändern: Ein Zustandswechsel ist ein Umbenennen, eine Datenänderung ist ein Neuschreiben mit Umbenennen (F3) — beides sauber getrennt.

**Naheliegend und verworfen.** *Eigener Wiederholungs-Ordner je Versuchsstufe* — vervielfacht die Zustände ohne Erkenntnisgewinn. *Wiederholung im Arbeitsspeicher des Arbeiters halten* — überlebt keinen Neustart und macht aus einer wartenden Wiederholung einen verlorenen Auftrag. *Unbegrenzt wiederholen* — ein dauerhaft unzustellbarer Auftrag beschäftigt den Arbeiter für immer; der Gescheitert-Ordner ist die Antwort darauf.

**Woran man merkt, dass es verletzt ist.** Nach einem Neustart sind wartende Wiederholungen verschwunden — oder der Arbeiter dreht sich an einem Auftrag fest.

### F7 · Wiederanlauf ist eine Aufräumregel, kein Verfahren

**Gilt für:** Erbauer.

**Was gilt.** Beim Start des Dienstes gilt genau zweierlei: Was im In-Arbeit-Ordner liegt, wird in den Wartend-Ordner zurückbenannt; was im Übergabeordner liegt, wird gelöscht. Mehr Wiederanlauf-Logik gibt es nicht.

**Warum.** Ein Auftrag im In-Arbeit-Ordner nach einem Neustart bedeutet genau eines: Der Dienst ist während seiner Bearbeitung gestorben. Ob er dabei schon etwas bewirkt hat, kann das Dateisystem nicht wissen — deshalb wird wiederholt, und deshalb muss die Ausführung so gebaut sein, dass eine Wiederholung verkraftbar ist. Dateien im Übergabeordner sind definitionsgemäß unfertig und damit wertlos. Beide Regeln sind in wenigen Zeilen umsetzbar und im Störfall auch von Hand nachvollziehbar.

**Naheliegend und verworfen.** *Ein Protokoll mitschreiben, um den Fortschritt genau zu rekonstruieren* — das ist der Anfang einer Datenbank, und sie hätte dieselbe Frage („war der letzte Schritt schon wirksam?") ebenfalls nicht beantwortet. *In-Arbeit-Aufträge nach Neustart als gescheitert werten* — verliert Aufträge, die nur unglücklich getimt waren.

**Woran man merkt, dass es verletzt ist.** Nach einem harten Neustart liegen Aufträge dauerhaft im In-Arbeit-Ordner und werden von niemandem mehr angefasst.

### F8 · Der Dienst löscht keine Nutzdaten

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Erledigtes und Gescheitertes bleiben liegen — das eine als Journal, das andere als Wartendes auf eine Entscheidung. Es gibt keine automatische Verfallszeit und keine Verbrauchssemantik für Nutzdaten. Automatisch geräumt wird ausschließlich der Übergabeordner (unfertige Reste älter als eine Stunde). Alles andere wird ausdrücklich gelöscht, durch den Aufrufer oder durch ein Kommando des Betreibers.

**Warum.** Diese Festlegung wurde erst nach einem verworfenen ersten Entwurf gefunden, und das ist ihr wichtigster Teil. Der erste Entwurf nahm an, hochgeladene Dateien seien Einweg-Artefakte: verbraucht, sobald der Auftrag erledigt ist, plus eine Verfallszeit für Verwaistes. Die Annahme ist falsch — ein Anhang wie ein Logo, ein Formblatt, eine Signatur wird einmal hochgeladen und von beliebig vielen späteren Aufträgen benutzt. Damit ist jede „verbraucht"- oder „verwaist"-Heuristik sinnlos, und jede Verfallszeit ist eine willkürlich gesetzte Zahl, die irgendwann Daten vernichtet, die jemand noch brauchte. Die ehrliche Regel lautet deshalb: Der Pool läuft voll, und der Betreiber räumt auf. Voll laufen ist ein sichtbares Problem; stilles Löschen ist keines — bis es eines ist.

**Naheliegend und verworfen.** *Verfallszeiten für Uploads* — willkürlich, und sie treffen die dauerhaft genutzten Artefakte. *Löschen nach Verwendung* — verkennt die Wiederverwendung. *Erledigtes automatisch verschlanken* — nimmt dem Betreiber genau das Journal weg, das er im Störfall braucht. *Größenbasierte Verdrängung* — wurde erwogen und bewusst auf eine spätere Fassung vertagt: Sie ist sinnvoll, aber sie braucht eine Quotenregelung, und die ist ein eigenes Thema.

**Woran man merkt, dass es verletzt ist.** Jemand sucht eine Datei, die es geben müsste, und findet die Begründung „automatisch entfernt".

### F9 · Sichtbarkeit statt Automatik — und nur nach innen

**Gilt für:** Erbauer · Betreiber.

**Was gilt.** Der Zustand der Warteschlange wird sichtbar gemacht, nicht automatisch behandelt: ein Kommando zeigt Bestände und Zähler, ein Kommando wiederholt einen gescheiterten Auftrag, ein Kommando verwirft ihn. Diese Auskunft gibt es **ausschließlich lokal** über die Kommandozeile; die Netzfläche des Dienstes gibt keine Bestandszahlen preis — ein Gesundheitspunkt sagt nur, ob der Dienst läuft.

**Warum.** Automatik entscheidet ohne Kenntnis der Lage; ein Betreiber mit einer Liste vor Augen entscheidet richtig. Und Bestandszahlen sind Betriebsinterna: Wer von außen ablesen kann, wie viel gerade wartet und wie viel scheitert, lernt etwas über Auslastung und Störungen, das ihn nichts angeht. Die Trennung kostet nichts, weil derjenige, der eingreifen darf, ohnehin auf dem Server ist.

**Naheliegend und verworfen.** *Bestandszahlen im Gesundheitspunkt* — bequem für Überwachung, aber es ist die Netzfläche, und sie ist offen. *Automatisches Wiederholen aller gescheiterten Aufträge nach einer Weile* — wiederholt auch das, was aus gutem Grund gescheitert ist. *Eine Weboberfläche für die Warteschlange* — eine Verwaltungsfläche mit allem, was daran hängt.

**Woran man merkt, dass es verletzt ist.** Ein Unbeteiligter kann von außen erkennen, wie viele Aufträge anliegen.

**Tragwerk (Kurzrekapitulation).** F3 trägt alles: Ohne die Atomizität des Umbenennens sind F4, F5 und F7 nicht mehr korrekt, sondern nur noch wahrscheinlich richtig. F1 und F2 sind die Form, in der das Muster überhaupt anwendbar wird — ein Auftrag, der nicht in eine Datei passt, und ein Zustand, der nicht im Ordner steht, brechen es sofort. F6 und F7 sind die Betriebshärte, F8 und F9 die Betriebskultur; beide letzteren sind unabhängig und ließen sich auch anders entscheiden, ohne das Muster zu verlieren.

## 5. Sicherheitsbetrachtung

| Angriff / Risiko | Typischer Hergang | Gegenmaßnahmen |
|---|---|---|
| Fremdzugriff über geratene Kennungen | Ein Aufrufer probiert Auftragskennungen durch | Kennungen sind kryptografisch zufällig, nicht fortlaufend (F1); zusätzlich physische Trennung: je Zugang ein eigener Verzeichnisbaum, sodass ein vergessener Filter fremde Aufträge gar nicht erst findet |
| Auskunft über fremde Existenz | Antwortverhalten verrät, ob eine Kennung existiert | Fremde wie unbekannte Kennungen werden gleich behandelt; die Verzeichnistrennung macht das zum Normalfall statt zur Sonderprüfung |
| Halb geschriebene Aufträge werden verarbeitet | Absturz während der Annahme | Übergabeordner plus Ausschreiben vor dem Umbenennen; unfertige Dateien sind nie im Ablauf sichtbar (F4) |
| Doppelverarbeitung | Zwei Arbeiter greifen denselben Auftrag | Der Anspruch ist das Umbenennen; nur einer kann gewinnen (F5) |
| Verlorene Aufträge nach Absturz | Dienst stirbt mit Aufträgen in Arbeit | Wiederanlauf-Regel benennt sie zurück (F7); die Ausführung muss eine Wiederholung verkraften |
| Preisgabe von Betriebsinterna | Bestandszahlen über die Netzfläche | Auskunft nur lokal über Kommandos (F9) |
| Volllaufen des Datenträgers | Nutzdaten werden nie automatisch gelöscht (F8) | Bewusst in Kauf genommen und sichtbar gemacht: Bestände zeigt ein Kommando; die Räumung ist eine Betreiberentscheidung. Wer das nicht tragen kann, braucht die vertagte Quotenregelung (§9) |
| Fremdzugriff auf den Datenträger | Wer den Baum lesen darf, liest alle Aufträge | Dateirechte sind die einzige Schutzschicht — es gibt keine Verschlüsselung im Ruhezustand; das ist eine bewusste Grenze (§7) |

Grenze des Modells: Die Warteschlange ist so gut geschützt wie das Verzeichnis, in dem sie liegt. Wer Leserechte auf den Baum hat, sieht alle Aufträge im Klartext. Das Muster ersetzt keine Zugriffskontrolle, es setzt sie voraus.

## 6. Was das kostet

**Jede Übersicht ist ein Verzeichnisdurchlauf (F2).** Es gibt keinen Index. „Wie viele Aufträge warten?", „Welche sind gescheitert?", „Welche gehören zu diesem Aufrufer?" — all das bedeutet, ein Verzeichnis zu lesen, im ungünstigen Fall mit jeder Datei darin. Bei Hunderten oder wenigen Tausend Aufträgen ist das unmerklich; bei Hunderttausenden wird es zum Problem, und zwar zuerst beim Betreiber, der nachsehen will.

**Das Dateisystem wird zur Betriebsvoraussetzung (F3).** Alles muss auf einem lokalen Dateisystem liegen. Kein Netzdateisystem, keine Verteilung über Maschinen, kein Ausweichen auf einen Objektspeicher. Wer später horizontal skalieren will, kann diese Warteschlange nicht mitnehmen.

**Viele kleine Dateien (F1).** Jeder Auftrag ist eine Datei, jede Wiederholung ein Neuschreiben. Das erzeugt Inode-Verbrauch und macht Sicherungen langsamer als eine einzelne Datenbankdatei — und Verzeichnisse mit sehr vielen Einträgen sind auf manchen Dateisystemen selbst ein Leistungsproblem.

**Der Betreiber muss aufräumen (F8).** Nichts verschwindet von selbst. Der Pool wächst, das Journal wächst, und irgendwann läuft der Datenträger voll. Das ist gewollt, aber es ist eine wiederkehrende Aufgabe, die jemand übernehmen muss.

**Kein Ereignisstrom, keine Prioritäten (F2, F6).** Die Warteschlange kann nur „ungefähr der Reihe nach". Vordrängeln, Prioritätsklassen, verzögerte Zustellung über lange Zeiträume, Fächer je Aufrufer — all das müsste man in Ordnernamen und Dateinamen hineincodieren, und ab dem zweiten solchen Wunsch baut man eine Datenbank nach.

**Keine Verschlüsselung im Ruhezustand (§5).** Die Aufträge liegen im Klartext. Wer das nicht darf, braucht eine andere Ablage — und damit ein anderes Muster.

## 7. Geltung und Grenzen

### Randbedingungen

**Ein lokales Dateisystem für den gesamten Baum.** Übergabe- und Zustandsordner müssen auf demselben Dateisystem liegen, sonst ist das Umbenennen kein Umbenennen mehr, sondern ein Kopieren mit Löschen (F3). Man erkennt die Lage an einer Einbindung mitten im Baum.

**Eine Größenordnung, in der Durchläufe billig bleiben.** Faustregel: Solange ein Verzeichnisdurchlauf für den Betreiber im Sekundenbereich bleibt, trägt das Muster. Das sind je nach Dateisystem einige Tausend bis einige Zehntausend Aufträge im Bestand — nicht im Durchsatz.

**Eine Ausführung, die Wiederholung verkraftet.** Weil der Wiederanlauf im Zweifel wiederholt (F7), muss die eigentliche Arbeit entweder wiederholbar oder von außen gegen Doppelwirkung gesichert sein.

### Wann die Antwort kippt

**Mehrere Maschinen sollen dieselbe Warteschlange bedienen.** Dann fällt F3, und mit ihm das ganze Muster. Netzdateisysteme sichern die Atomizität nicht durchgängig zu; wer verteilen muss, braucht eine echte Warteschlangen-Infrastruktur.

**Abfragen über Merkmale werden zum Alltag.** Sobald regelmäßig „alle Aufträge mit Eigenschaft X" gefragt wird, fehlt ein Index — und ein Index neben dem Verzeichnisbaum ist die zweite Wahrheit, die F2 gerade vermeidet. Das ist der Punkt, an dem eine eingebettete Datenbank die ehrlichere Antwort wird.

**Der Durchsatz erreicht die Dateisystem-Grenze.** Bei sehr hohen Raten wird das Anlegen und Umbenennen vieler kleiner Dateien selbst zum Engpass, und Verzeichnisse mit sehr vielen Einträgen werden langsam.

**Die Aufträge dürfen nicht im Klartext liegen.** Verschlüsselung im Ruhezustand ist in diesem Muster nicht vorgesehen; nachrüsten hieße, jede Datei einzeln zu ver- und entschlüsseln und die Inspizierbarkeit zu verlieren — also genau den zweiten Hauptvorteil.

### Was es nicht löst

**Zugriffskontrolle.** Das Muster setzt voraus, dass die Dateirechte stimmen; es stellt sie nicht her.

**Genau-einmal-Ausführung.** Nach einem Absturz wird wiederholt, ohne zu wissen, ob die vorige Ausführung schon gewirkt hat. Wer Genau-einmal braucht, braucht eine Absprache mit dem Zielsystem — das Muster liefert Mindestens-einmal.

**Prioritäten, Fächer, Zeitpläne.** Alles, was über „ungefähr der Reihe nach, mit Wiederholung" hinausgeht.

**Aufbewahrung und Sicherung.** Wie lange das Journal geführt wird und wie der Baum gesichert wird, ist Betriebsarbeit außerhalb des Musters.

## 8. Bewährung

**Herkunft: das Maildir-Format.** Das Muster ist eine Übertragung des Maildir-Formats aus der Mailserver-Welt, entstanden Ende der 1990er Jahre als sperrfreie Alternative zum mbox-Format: Eine Nachricht ist eine Datei, sie entsteht in `tmp`, wird nach `new` umbenannt und später nach `cur` — genau die drei Rollen, die hier Übergabe, Wartend und Erledigt heißen. Dass das Verfahren jahrzehntelang unter Last funktioniert hat, ist der eigentliche Beleg für seine Tragfähigkeit; dieses Dossier überträgt es nur auf die Auftragswarteschlange eines kleinen Dienstes und schreibt die Betriebsregeln dazu.

**Referenzimplementierung (Stand offen).** Der Entwurfsablage-Dienst des Autors setzt das Muster vollständig um: Verzeichnisbaum je Zugang mit den fünf Zustandsordnern, Zufallskennung mit vorangestelltem Zeitstempel, Wiederholung mit Zeitpunkt in der Datei, Wiederanlauf-Regel beim Start, Journal ohne Auto-Räumung, Bestandsanzeige und Eingriffe ausschließlich über Kommandos. Ergänzt ist er um zwei Dinge, die zum Muster gehören, aber über die reine Warteschlange hinausgehen: einen getrennten, dauerhaften Dateipool für Anhänge (auf den Aufträge nur verweisen, F8) und Idempotenz-Marker, die per exklusivem Anlegen verhindern, dass eine wiederholt eingereichte Anfrage zwei Aufträge erzeugt. *Die Implementierung lief zum Zeitpunkt dieser Fassung; eine Vollzugsmeldung steht aus und ist hier nachzutragen.*

**Erwogen und im Vergleich unterlegen.** Vor der Entscheidung stand eine eingebettete Datenbank zur Debatte. Ihr einziges eigenständiges Argument war der Zustands-Abruf für den Aufrufer („was ist aus meinem Auftrag geworden?") — und der funktioniert über den Ordner-Ort genauso gut. Dagegen standen: Nebenläufigkeitsverhalten, das man verstehen und testen muss; ein Schema mit Wanderungsgeschichte; eine Abhängigkeit im Auslieferungserzeugnis; und der Verlust der Inspizierbarkeit mit Bordmitteln. Die Entscheidung fiel deshalb gegen jede Datenbank im System.

## 9. Offene Fragen

**Q1** *(Auftrag an den Verwender)* Welche Wiederholungsstaffel gilt, und wann gilt ein Auftrag als endgültig gescheitert? Das hängt vom Zielsystem ab; das Muster verlangt nur, dass beides festgelegt und dokumentiert ist.

**Q2** *(Auftrag an den Verwender)* Ab welchem Bestand wird die Warteschlange unhandlich, und was passiert dann? Der Schwellwert ist dateisystem- und einsatzabhängig; ihn zu kennen, bevor er erreicht wird, ist Teil der Übernahme.

**Q3** Quoten und größenbasierte Verdrängung wurden bewusst vertagt (F8). Sollten sie als Festlegung nachgezogen werden, und wenn ja: je Zugang, je Dienst, nach Alter oder nach Größe?

**Q4** Gehört die Aufbewahrungsdauer des Journals in dieses Muster oder in den Betrieb? Ohne Regel wächst es unbegrenzt; mit Regel widerspricht es F8 dem Buchstaben nach.

**Q5** Braucht das Muster eine Aussage zur Sicherung? Ein konsistenter Abzug eines Baums, in dem gerade umbenannt wird, ist nicht selbstverständlich — vielleicht genügt der Hinweis, dass ein Abzug im laufenden Betrieb Aufträge doppelt oder gar nicht enthalten kann.

**Q6** Wie verhält sich das Muster zu Dateisystemen mit abweichender Umbenenn-Semantik oder zu Containern mit Schichtdateisystemen? Erwartet wird, dass ein Datenträger eingebunden sein muss — eine ausdrückliche Aussage fehlt.

## 10. Quellen & Verwandtes

*Zur Fertigstellung: Die folgenden Angaben sind aus der Entwurfsarbeit übernommen; Fundstellen sind vor der Veröffentlichung zu prüfen und mit Abrufdatum zu versehen.*

**Quelle 1 · D. J. Bernstein: Maildir-Format (qmail).** Das Ursprungsmuster: eine Nachricht je Datei, `tmp`/`new`/`cur`, sperrfrei durch atomares Umbenennen. — *Wofür sie hier steht:* Herkunft und Bewährungsnachweis des Verfahrens (§8).

**Quelle 2 · POSIX-Spezifikation von `rename()`.** Die Atomizitätszusage innerhalb eines Dateisystems, auf der F3 ruht. — *Wofür sie hier steht:* Die technische Grundlage, deren Grenzen (Dateisystemgrenzen, Netzdateisysteme) die Randbedingungen in §7 bestimmen.

**Quelle 3 · Entwurfsablage-Dienst des Autors, Repository `imap-service`.** — *Wofür sie hier steht:* Die Referenzimplementierung aus §8.

**Quelle 4 · Schwester-Konzept dieser Reihe: `service-as-single-artifact`.** — *Wofür sie hier steht:* Der Zuschnitt des Dienstes, in dem diese Warteschlange wohnt — insbesondere die Trennung von Konfiguration und Betriebsdaten, zu denen der Warteschlangenbaum gehört. Vertiefung, kein Verständnis-Muss.
