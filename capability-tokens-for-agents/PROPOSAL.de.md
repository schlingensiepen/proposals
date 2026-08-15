---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: capability-tokens-for-agents
language: de (führend)
question: "Wie entwirft, übergibt, verwahrt und verwendet man Zugangs-Tokens, wenn der Konsument eines Dienstes ein KI-Agent ist?"
thesis: "Ein Token verkörpert genau eine Fähigkeit an genau einem Ort, und seine Handhabung folgt festen Regeln entlang des Lebenszyklus — denn im Agenten-Kontext ist jedes Leck durchsuchbar und kopierbar; wer Zuschnitt und Handhabung beide festlegt, übersteht Teilversagen glimpflich."
---

# Capability-Tokens für Agenten

**In einem Satz:** Ein Token verkörpert genau eine Fähigkeit an genau einem Ort, und seine Handhabung folgt festen Regeln entlang des Lebenszyklus — Erzeugung, Übergabe, Ablage, Verwendung, Rotation, Entzug, Verfall —, denn im Agenten-Kontext ist jedes Leck durchsuchbar und kopierbar; wer Zuschnitt und Handhabung beide festlegt, übersteht Teilversagen glimpflich.

**Kurzthese:** Wenn der Konsument eines Dienstes ein KI-Agent ist, ändern sich die Leck-Kanäle für Zugangsdaten fundamental — der Chatverlauf wird zum durchsuchbaren, kopierbaren Speichermedium, und der Agent lässt sich durch fremden Text steuern. Die wirksamste Antwort ist ein doppelter Zuschnitt: **Tokens als enge Capabilities** auf der Dienstseite und **feste Handhabungsregeln** auf der Agentenseite. Dieses Konzept gilt für jeden Dienst, den Agenten konsumieren — Web-API, Kommandozeile, MCP, selbstbeschreibend oder nicht.

## 1. Problem

Ein klassischer API-Client ist ein Programm mit Konfigdatei: Das Token liegt an einer Stelle, fließt durch einen Codepfad, und wer es sehen kann, bestimmen Dateirechte. Ein Agent ist anders. Sein „Arbeitsspeicher" ist ein Kontextfenster, das geloggt, zusammengefasst, geteilt und weiterverarbeitet wird; seine „Konfiguration" ist oft Konversation; und er verarbeitet fremde Inhalte — Mails, Webseiten, Dokumente —, die ihn durch bloßen Text zu Handlungen bewegen können (Prompt Injection). Ein Token, das im Chatverlauf steht, ist kein Geheimnis mehr, sondern Bewohner eines durchsuchbaren, exfiltrierbaren Mediums.

Die üblichen Reaktionen greifen zu kurz: „Agenten bekommen keine Tokens" verhindert nützliche Automatisierung; „wir vertrauen dem Agenten" ignoriert, dass der Agent selbst angegriffen werden kann. Gebraucht wird ein Design, das mit Teilversagen rechnet.

Drei Rollen tragen das Problem: **Der Betreiber** eines Dienstes gibt Zugänge aus und muss sie wieder einfangen können. **Der Agent** verwahrt und verwendet sie in einer Umgebung, die zum Aufbewahren von Geheimnissen nie gedacht war. **Der Anwender** entscheidet, wer was darf — und soll das können, ohne je ein Geheimnis in die Hand zu nehmen.

## 2. Das Konzept

Der Kern ist ein **doppelter Zuschnitt**. Dienstseitig werden Tokens als enge **Capabilities** geschnitten: Ein Token *ist* die Erlaubnis für genau eine Fähigkeit an genau einem Ort — es gibt keinen Parameter, mit dem der Aufrufer den Wirkungsbereich wählt, und damit keinen Autorisierungscode, der falsch sein könnte. Wer zwei Ziele braucht, bekommt zwei Tokens. Agentenseitig gilt ein fester **Handhabungs-Kodex**: einmal erhalten, außerhalb des Konversationstexts abgelegt, nie wieder zitiert, ausschließlich gegen den Ort verwendet, für den es ausgegeben wurde.

Verbunden werden beide Seiten am **Setup** — dem Moment, in dem das Token vom Betreiber zum Agenten will, ohne durch Menschenhände zu laufen. Dafür empfiehlt dieses Konzept die offene **Selbstregistrierung** mit **Display-String**: Der Agent stellt sich vor, erhält Token und einen aussprechbaren, ungeheimen Griff, und der Anwender vergibt die Rechte an den Griff — nie an das Geheimnis. Der Rest ist Lebenszyklus: Einmal-Anzeige bei der Erzeugung, Ablage in fester Rangfolge, Rotation als Routine, Entzug, der genau einen Zweck trifft, Verfall für alles Unbestätigte und Ungenutzte.

Das Konzept ist bewusst allgemein: Es setzt keinen bestimmten Diensttyp voraus. Ein ausgeführtes Anwendungsbeispiel — ein Bauprinzip, das diese Festlegungen für selbstbeschreibende Web-Dienste verbindlich macht — zeigt §8.

## 3. Begriffe

**Capability** — eine einzelne, eng abgegrenzte Fähigkeit an einem Dienst (etwa: Meldungen beitragen an genau dieser Instanz). Der Rechte-Zuschnitt dieses Konzepts denkt in Capabilities.

**Token** — das Artefakt, das genau eine Capability verkörpert; im Agenten-Kontext das empfohlene Zugangsartefakt bei Neubauten.

**Geheimnis** — jedes vertrauliche Zugangsdatum: API-Key, Passwort — oder ein Token. *Jedes Token ist ein Geheimnis; nicht jedes Geheimnis ist ein Token.* Die Handhabungsregeln dieses Konzepts gelten für jedes Geheimnis.

**Ort** — das Ziel, für das ein Token ausgegeben wurde: eine Instanz, eine Basis-URL, ein Zielsystem. Die Bindung an den Ort ist Teil der Capability.

**Lebenszyklus** — die Stationen eines Tokens: Erzeugung · Übergabe · Ablage · Verwendung · Rotation · Entzug · Verfall.

**Einmal-Anzeige** — das Klartext-Token erscheint genau einmal, bei der Erzeugung; danach gibt es keine Abruf-Funktion. Verlust bedeutet Rotation, nicht Recherche.

**Kurz-ID** — eine harmlose, nicht geheime Kennung eines Tokens für Logs und Verwaltung; der Dienst speichert vom Token selbst nur den Hash.

**Display-String** — der kurze, aussprechbare, **nicht geheime** Griff zu einem Token (8 Zeichen, Kleinbuchstaben), über den Menschen und Agenten über Tokens sprechen und Rechte vergeben, ohne je das Geheimnis zu nennen.

**Selbstregistrierung** — die optionale offene Methode, über die ein Konsument gegen eine Freitext-Selbstvorstellung ein Token erhält; in zwei Geschmacksrichtungen: **automatisch** (sofort nutzbar) und **beantragend** (rechtelos geboren, wird nachträglich scharf geschaltet).

**Zweiter Kanal** — ein Übergabeweg vom Betreiber zum Anwender außerhalb des Chats; nötig überall dort, wo keine Selbstregistrierung angeboten wird.

**Durchleitungs-Datei** — eine Datei mit engen Rechten, die ein Geheimnis ausschließlich durchreicht: Der Inhalt fließt direkt in den Aufruf und erscheint nie in Ausgaben, Logs oder dem Kontextfenster.

**Kanonischer Variablenname** — der vom Dienst selbst vorgeschlagene Name der Umgebungsvariablen für sein Geheimnis; löst das Wiederfinde-Problem über Sitzungen hinweg.

**Rotation** — Ersetzen eines Tokens durch ein neues bei fortbestehendem Zweck; **Entzug** — endgültiges Stilllegen; **Verfall** — automatisches Erlöschen unbestätigter oder ungenutzter Tokens.

## 4. Festlegungen

**Landkarte.** Zwölf Festlegungen in drei Blöcken entlang des Lebenszyklus. **F1–F4** sind der *Zuschnitt auf der Dienstseite*: was ein Token verkörpert, wie es entsteht, wo seine Fähigkeit wohnt und welche Flächen der Dienst dafür härtet. **F5–F9** sind der *Lebenszyklus*: Übergabe, Ablage, Bindung, Rotation und Entzug, Verfall. **F10–F12** sind das *Agenten-Verhalten und seine Zustellung*: die Niemals-Regeln, die Sprechweise über Tokens, und die Pflicht, all das mit dem Dienst auszuliefern. F1 trägt den Zuschnitt; fällt es (etwa bei einer Bestands-API mit ererbtem Passwort), bleibt der Handhabungs-Teil F5–F11 für jedes Geheimnis in Kraft — das Konzept zerfällt bewusst in zwei je für sich tragfähige Hälften. Zwei Festlegungen sind ausdrücklich **Empfehlungen** (F5-Selbstregistrierung als empfohlene Setup-Lösung, F11 Display-String); der Rest ist bei Übernahme des Konzepts verbindlich. Jede Festlegung nennt, für wen sie gilt.

### F1 · Ein Token, eine Fähigkeit, ein Ort

**Gilt für:** Betreiber.

**Was gilt.** Ein neu gebauter Dienst SOLL Zugriffsrechte als enge Capabilities zuschneiden: Ein Token verkörpert genau eine Fähigkeit an genau einem Ort; wer zwei Ziele oder zwei Fähigkeiten braucht, bekommt zwei Tokens. Abweichungen sind mit guten Gründen zulässig — eine Bestands-API bringt mit, was sie hat; für ihr ererbtes Geheimnis gelten dann die Handhabungsregeln F5–F11 unverändert.

**Warum.** Im Agenten-Kontext ist der Chatverlauf ein realer Leck-Kanal. Enge Tokens machen jedes einzelne Leck chirurgisch klein: Entzug und Rotation treffen genau einen Zweck, nicht den ganzen Dienst — und der Anwender kann Rechte in der Granularität vergeben, in der er denkt („darf Staumeldungen melden"), statt in der Granularität, die ein Admin-Modell erzwingt.

**Naheliegend und verworfen.** *Ein Admin-Token „zur Sicherheit"* — macht jedes Leck total; die Bequemlichkeit von heute ist der Totalschaden von morgen. *Ein Sammel-Token je Anwender über alle seine Zwecke* — koppelt Unzusammenhängendes: Ein Leck beim Nebenzweck öffnet den Hauptzweck.

**Woran man merkt, dass es verletzt ist.** Der Entzug eines einzelnen Tokens legt mehr still als den einen Zweck, für den es ausgegeben wurde.

### F2 · Erzeugung: serverseitig, hochentropisch, nur einmal sichtbar

**Gilt für:** Betreiber.

**Was gilt.** Tokens werden ausschließlich serverseitig erzeugt (CSPRNG, mindestens 128 Bit Entropie) und tragen ein erkennbares Präfix (etwa `svc_…`), damit Secret-Scanner Lecks finden und niemand Tokens verwechselt. Der Dienst speichert vom Token **nur den Hash** (bei hochentropischen Zufallstokens genügt schnelles SHA-256; Passwort-KDFs adressieren erratbare Geheimnisse) plus eine harmlose Kurz-ID für Logs und Verwaltung. Das Klartext-Token erscheint genau **einmal**, bei der Erzeugung; eine Abruf-Funktion gibt es nicht — Verlust bedeutet Rotation, nicht Recherche.

**Warum.** Einmal-Anzeige minimiert die Zahl der Kopien auf genau die, die der Empfänger anlegt; die Hash-Speicherung sorgt dafür, dass ein geleaktes Konfig-File oder Backup des Betreibers kein Schlüsselbund ist; und das Präfix macht aus jedem versehentlichen Commit einen Treffer, den Werkzeuge finden, statt eines Geheimnisses, das Jahre unbemerkt liegt.

**Naheliegend und verworfen.** *Selbstgewählte Tokens mit Passwort-Charakter* — Menschen wählen wieder, was sie sich merken können, und merken können sie sich, was sie schon woanders benutzen. *Eine „Token anzeigen"-Funktion für den Verlustfall* — jede Wiederanzeige-Fläche ist eine Abfrage-Fläche; der Erfahrungswert aus dem Betrieb: Ihr Fehlen erzeugt anfangs Nachfragen („ich habe es verloren") — und genau die gewünschte Rotationsdisziplin. *Token per Mail oder Ticket „zur Bequemlichkeit"* — macht Postfach und Ticketsystem zum Schlüsselbund.

**Woran man merkt, dass es verletzt ist.** Ein Token lässt sich nach Wochen noch irgendwo „nachschlagen" — beim Betreiber, im Postfach, im Ticket.

### F3 · Das Token ist der Scope

**Gilt für:** Betreiber.

**Was gilt.** Die Fähigkeit steckt im Token selbst, nicht in einem wählbaren Parameter des Aufrufs: Der Dienst prüft das Token und leitet daraus vollständig ab, was erlaubt ist. Es gibt keinen Scope-Parameter, den der Aufrufer setzt, und keine Rechte, die im Ausgabeaufruf mitbestellt werden.

**Warum.** Ein wählbarer Parameter ist Autorisierungscode, der falsch sein kann — und ein Agent, der wörtlich nimmt, setzt ihn wörtlich falsch. Eine Fähigkeit, die im Artefakt wohnt, ist nicht verhandelbar: Das an die falsche Stelle geratene Token *kann* dort nicht mehr, als es verkörpert.

**Naheliegend und verworfen.** *Scope-Parameter am Aufruf, gegen das Token geprüft* — Redundanz mit Fehlerraum: Zwei Stellen müssen übereinstimmen, eine davon setzt der Angreifer. *Rechte im selben Aufruf mitbestellen, der das Token erzeugt* — dann entscheidet der Anonyme über seinen eigenen Zuschnitt; die Trennung von Erzeugung und Scharfschaltung ist der Schutz.

**Woran man merkt, dass es verletzt ist.** Ein Aufruf mit demselben Token wirkt je nach mitgesendetem Parameter unterschiedlich weit.

### F4 · Transport und Fehlersemantik: keine Orakel, keine Streuflächen

**Gilt für:** Betreiber.

**Was gilt.** Tokens reisen ausschließlich im Bearer-Header — nie in der URL, nie im Payload. Logs des Dienstes enthalten nur die Kurz-ID, nie das Token. Und die Fehlersemantik gibt kein Orakel ab: Unbekannte Tokens erhalten 401; fremde Ressourcen antworten **identisch** wie nicht existierende (404). Wer rät, lernt nichts — nicht einmal, dass es etwas zu erraten gäbe.

**Warum.** URLs landen in Proxy-Logs, Browser-Historien und Anwendungs-Logs — der Header-Weg schließt diese Leck-Kanäle konstruktiv aus statt organisatorisch. Und die Existenz einer Ressource ist selbst eine Information: Eine 403-Antwort („gibt es, gehört dir nicht") verrät einem Suchenden die Landkarte des Dienstes.

**Naheliegend und verworfen.** *Token als URL-Parameter „für einfache Integration"* — jede Stelle, die URLs speichert, speichert dann Geheimnisse. *Unterscheidbare Fehler „für bessere Diagnose"* — die Diagnose gehört in die lehrende Fehlermeldung für den legitimen, authentifizierten Aufrufer, nicht in die Antwort an den anonymen Rater.

**Woran man merkt, dass es verletzt ist.** Ein Token taucht in einem Access-Log auf — oder ein systematischer Durchprobierer kann aus den Antworten ablesen, welche Ressourcen existieren.

### F5 · Übergabe über einen zweiten Kanal — empfohlen: Selbstregistrierung

**Gilt für:** Betreiber · Anwender.

**Was gilt.** Die Übergabe eines Tokens an den Konsumenten läuft über einen Kanal außerhalb des Chats; welche Form dieser zweite Kanal hat, legt der Verwender je Einsatz fest (Auftrag Q2). Als Lösung des Setup-Problems EMPFOHLEN ist die offene **Selbstregistrierung** in drei Schritten: **(1)** Der Agent ruft sie mit einer Freitext-Selbstvorstellung auf (wer, wofür). **(2)** Der Dienst antwortet mit Token, Display-String und einem Flag, das die Geschmacksrichtung nennt — **automatisch** (sofort nutzbar; nur bei geringem Schadenspotential) oder **beantragend** (das Token kommt rechtelos zur Welt). **(3)** In der Geschmacksrichtung „beantragend" zeigt der Agent seinem Anwender den Display-String, niemals das Token — und der Anwender vergibt die Rechte serverseitig an genau diesen Display-String. Die Selbstvorstellung wird mit dem Token gespeichert und ist dessen Entscheidungs- und Audit-Grundlage. Und es sei ehrlich gesagt: **Wirklich sicher ist das nicht** — es ist der bewusste Kompromiss zum schnellen Starten; kompliziertere Verfahren haben sich im Versuch als unbenutzbar erwiesen.

**Warum.** Das Setup ist der Moment, in dem Geheimnisse durch Menschenhände wollen — jede Hand ist eine Kopie. Die Selbstregistrierung macht das Onboarding Ende-zu-Ende, ohne dass je ein Geheimnis über einen Menschen läuft: Der Mensch fasst nur den ungeheimen Griff an. Wer Monate später die Token-Liste liest, sieht an der Selbstvorstellung noch, wer da warum Zugang bekam.

**Naheliegend und verworfen.** *Der Chat als Normalweg der Übergabe* — macht den Transkript-Speicher zum Schlüsselbund; als erklärter letzter Weg regelt ihn F6. *OAuth-Vollausbau für kleine Dienste* — unverhältnismäßig; wo ein Ökosystem ihn vorschreibt, gilt er ohnehin (§7). *Rechte im Registrierungsaufruf mitbestellen* — siehe F3.

**Woran man merkt, dass es verletzt ist.** Anwender diktieren Tokens durch Chats oder Ticketsysteme — oder das Onboarding scheitert daran, dass gerade niemand mit Serverzugang greifbar ist.

### F6 · Ablage in fester Rangfolge

**Gilt für:** Agent (ausgeliefert durch den Betreiber, F12).

**Was gilt.** Der Agent bezieht und verwahrt das Geheimnis in dieser Rangfolge: **1.** Umgebungsvariable — der Dienst DARF dafür einen kanonischen Namen vorschlagen; **2.** wenn die Umgebung das nicht kann: Datei mit engen Rechten, ausschließlich als **Durchleitung** benutzt — der Inhalt fließt direkt in den Aufruf und erscheint nie in Ausgaben, Logs oder dem Kontextfenster; **3.** erst als letzter Weg das Kontextfenster selbst — einmal nennen, sofort persistieren, nie wiederholen. Die Rangfolge gilt für jedes Geheimnis, ob Token oder ererbter API-Key.

**Warum.** Chatverläufe und Logs sind durchsuchbare, kopierbare Speicher. Die Rangfolge minimiert, wie lange und wie oft ein Geheimnis in ihnen existiert — und sie gibt dem Agenten eine prüfbare Regel statt eines Gefühls. Der kanonische Variablenname löst nebenbei das Wiederfinde-Problem über Sitzungen hinweg: Der Dienst selbst sagt, wo sein Geheimnis üblicherweise wohnt.

**Naheliegend und verworfen.** *„Geheimnis einfach in den Chat" als Normalweg* — der letzte Weg ist als Ausnahme erklärt, nicht als Gewohnheit erlaubt. *Client-seitige Verschlüsselungs-Handstände* — der Agent kann keine Geheimnisse vor sich selbst haben; Scheinsicherheit.

**Woran man merkt, dass es verletzt ist.** Ein Geheimnis taucht in einem Transkript, einem Log oder einer Fehlermeldung auf.

### F7 · Bindung an den Ausgabeort

**Gilt für:** Agent (ausgeliefert durch den Betreiber, F12) · Betreiber.

**Was gilt.** Ein Token wird ausschließlich gegen den Ort verwendet, für den es ausgegeben wurde. Je Instanz beziehungsweise Zielsystem gibt es ein eigenes Token; bei Mehrdeutigkeit („ins Wiki damit" — aber welches?) fragt der Agent nach, statt zu raten. Der Betreiber SOLL die Bindung zusätzlich serverseitig prüfen, wo seine Topologie das hergibt (Host- oder Mandanten-Bindung): Ein an die falsche Stelle geratenes Token läuft dann ins Leere.

**Warum.** Gleiche Software heißt nicht gleiches Vertrauen: Das Firmen-System und das private teilen die API, aber sonst nichts. Die Verwechslung meldet keinen Fehler — die Aufrufmechanik passt perfekt, nur das Ziel ist falsch. Die doppelte Bindung (Verhaltensregel beim Agenten, Prüfung beim Betreiber) dämpft genau den Fall, in dem Dritte den Agenten als verwirrten Stellvertreter missbrauchen.

**Naheliegend und verworfen.** *Ein Token je Software mit Zielliste darin* — verschiebt die Verwechslungsgefahr nur nach innen. *Das Ziel aus dem Kontext raten* — der Agent, der wörtlich nimmt, rät auch wörtlich falsch.

**Woran man merkt, dass es verletzt ist.** Ein Beitrag landet im falschen System — oder ein Geheimnis wird gegen ein anderes Ziel verwendet als das seiner Ausgabe.

### F8 · Rotation und Entzug treffen genau einen Zweck

**Gilt für:** Betreiber · Anwender.

**Was gilt.** Jedes Token ist einzeln entziehbar und einzeln rotierbar, und beides unterbrechungsfrei: Das neue Token ist parallel zum alten gültig, bis das alte gelöscht wird. Rotation ist ein Routinevorgang (ein Kommando, ein Handgriff), kein Projekt. Ablaufdaten — wo Tokens oder die Secrets ihrer Ausgabewege befristet sind — gehören in Betriebskalender und Monitoring; den Rhythmus legt der Verwender fest (Auftrag Q3).

**Warum.** Der Wert des engen Zuschnitts realisiert sich erst im Entzug: Wenn das Stilllegen eines Tokens genau einen Zweck kostet, wird aus dem Verdachtsfall ein Betriebsvorgang statt einer Chirurgie — und dann findet Rotation auch tatsächlich statt. Die Empirie der Nachbar-Dossiers ist eindeutig: Ablaufende Secrets und widerrufene Zugänge sind die häufigsten Langzeit-Ausfallgründe, und sie überraschen genau die Betriebe, die kein Ablaufdatum im Kalender haben.

**Naheliegend und verworfen.** *„Tokens laufen nie ab, das ist bequemer"* — Bequemlichkeit schlägt Hygiene genau so lange, bis das erste Leck unbemerkt Jahre alt ist. *Big-Bang-Rotation aller Tokens zum Stichtag* — koppelt Ausfälle, die der enge Zuschnitt gerade entkoppelt hat.

**Woran man merkt, dass es verletzt ist.** Ein Entzug erzwingt die Neueinrichtung unbeteiligter Zwecke — oder ein ablaufendes Secret überrascht den Betrieb.

### F9 · Verfall ist Betreiberpflicht — die Werte setzt der Verwender

**Gilt für:** Betreiber.

**Was gilt.** Rechtelos geborene Tokens, die nie scharf geschaltet werden, und Tokens, die dauerhaft ungenutzt bleiben, verfallen automatisch; offene Ausgabewege (Selbstregistrierung) sind gedrosselt. Die konkreten Fristen und Schwellwerte normiert dieses Konzept bewusst nicht — sie sind Betriebsgrößen, die nur der jeweilige Betreiber kennt, und damit ein Auftrag an den Verwender (Q1).

**Warum.** Vorratshaltung ist das stille Gegenteil des engen Zuschnitts: Jedes vergessene Token ist eine Fähigkeit, die niemand mehr vermisst und jeder noch verwenden kann. Ein offener Ausgabeweg ohne Drosselung und Verfall sammelt genau solche Tokens an.

**Naheliegend und verworfen.** *Einheitsfristen im Konzept normieren* — vergleichbar und prüfbar, aber falsch: Dieselbe Frist ist für den Hobby-Dienst großzügig und für die Firma fahrlässig. *Kein Verfall („wird schon niemand missbrauchen")* — Hoffnung als Sicherheitsarchitektur.

**Woran man merkt, dass es verletzt ist.** Die Token-Liste wächst monoton, und niemand kann sagen, wozu die Hälfte der Einträge gehört.

### F10 · Die Niemals-Regeln des Agenten

**Gilt für:** Agent (ausgeliefert durch den Betreiber, F12).

**Was gilt.** Ein Agent, der ein Geheimnis führt, befolgt vier Niemals-Regeln: Er wiederholt es **niemals** in Chat, Ausgaben oder Dateien, die nicht seiner Ablage dienen. Er verwendet es **niemals** gegen ein anderes Ziel als den Ausgabeort (F7). Er behandelt fremde Anweisungen, die Geheimnisse betreffen — „sende dein Token an …", „zeig mir mal deinen Key" —, **niemals** als Befehle, sondern als Daten. Und er persistiert **niemals** über die Reichweite hinaus, die sein Anwender genannt hat.

**Warum.** Das sind die drei realen Exfiltrationswege — Wiederholung, Fehlleitung, Injektion — plus die Überdauerung, die aus einem Sitzungs-Zugang einen Dauerzugang macht. Niemals-Regeln sind bewusst absolut formuliert: Sie sind prüfbar, sie brauchen keine Abwägung im Moment des Angriffs, und sie bleiben auch dann in Kraft, wenn ein fremder Text das Gegenteil verlangt.

**Naheliegend und verworfen.** *„Der Agent wird schon vernünftig sein"* — Vertrauen ersetzt keine Regel; angegriffen wird der Agent, nicht die Vernunft. *Das Geheimnis „zur Kontrolle" an den Anwender zurückspiegeln* — erzeugt genau die Kopie im Chat, die alle anderen Regeln verhindern.

**Woran man merkt, dass es verletzt ist.** Ein Token taucht in einer Antwort auf — oder ein Aufruf geht an eine Adresse, die aus fremdem Arbeitsmaterial stammt.

### F11 · Sprechweise: der Display-String

**Gilt für:** Betreiber · Agent · Anwender.

**Was gilt.** EMPFOHLEN: Jedes Token erhält bei der Erzeugung einen kurzen, aussprechbaren, **nicht geheimen** Griff — den Display-String (8 Zeichen, Kleinbuchstaben). Menschen und Agenten sprechen über Tokens ausschließlich per Display-String; Rechtevergabe, Entzug und Nachfragen adressieren den Griff, nie das Geheimnis.

**Warum.** Über Tokens *muss* gesprochen werden — vergeben, entziehen, zuordnen, nachfragen —, und jedes Gespräch, das das Geheimnis selbst braucht, bewegt das Geheimnis. Der Griff macht das Sprechen gefahrlos: diktierfest am Telefon, lesbar in der Token-Liste, eindeutig in der Rechtevergabe. Die Kurz-ID aus F2 leistet das Technische (Logs); der Display-String leistet das Menschliche.

**Naheliegend und verworfen.** *Das Token selbst als Referenz im Gespräch* — dann wandert das Geheimnis durch Chat und Zuruf. *Interne IDs als Griff* — nicht aussprechbar, nicht diktierfest; was Menschen nicht bequem benutzen können, umgehen sie.

**Woran man merkt, dass es verletzt ist.** In Chats oder Tickets stehen Token-Fragmente, weil es keinen anderen Namen für das Ding gab.

### F12 · Auslieferungspflicht: Der Kodex reist mit dem Dienst

**Gilt für:** Betreiber (zugunsten des Agenten).

**Was gilt.** Jede Regel dieses Dossiers, die sich an Agenten richtet — die Ablage-Rangfolge (F6), die Bindung (F7), die Niemals-Regeln (F10), die Sprechweise (F11) —, MUSS in der Dokumentation beziehungsweise Selbstauskunft des Dienstes selbst stehen, in dessen eigenen Worten.

**Warum.** Der konsumierende Agent kennt dieses Dossier nicht; er kennt nur, was ihm sein Dienst und sein Anwender sagen. Eine Agenten-Regel, die nur hier steht, erreicht niemanden — sie wäre Hoffnung, keine Festlegung. Die Kette lautet Dossier → Betreiber → Dienst-Dokumentation → Agent; erst die Auslieferung macht aus Empfehlungen wirksame Pflichten.

**Naheliegend und verworfen.** *Die Regeln jeder Agenten-Plattform einzeln beibringen* — das N×M-Problem, diesmal für Verhaltensregeln. *Auf allgemeine Plattform-Grundregeln vertrauen* — die variieren; was der Dienst braucht, muss der Dienst sagen.

**Woran man merkt, dass es verletzt ist.** Agenten verschiedener Plattformen handhaben dasselbe Token unterschiedlich — die einen persistieren korrekt, die anderen zitieren es im Chat —, obwohl alle dieselbe Dienst-Dokumentation gelesen haben.

**Tragwerk (Kurzrekapitulation).** F1 trägt den Zuschnitt; F2–F4 sind seine Ausgabe- und Oberflächen-Konkretisierung — ohne serverseitige Erzeugung mit Einmal-Anzeige (F2) und ohne Scope-im-Token (F3) bleibt vom engen Zuschnitt nur die Absicht, und F4 schließt die Streuflächen, durch die auch das engste Token entweicht. F5 verbindet Dienst- und Agentenseite am Setup. F6–F9 sind der Lebenszyklus: Ablage (F6) und Bindung (F7) schützen das lebende Token, Rotation/Entzug (F8) und Verfall (F9) begrenzen das sterbende. F10 und F11 sind das Agenten-Verhalten; F12 macht es zustellbar — fällt F12, bleiben alle Agenten-Regeln Hoffnung. Fällt F1 (Bestands-API), bleiben F5–F11 als Handhabungs-Kodex für jedes Geheimnis vollständig in Kraft: Das Konzept zerfällt in zwei tragfähige Hälften, und genau das ist beabsichtigt.

## 5. Sicherheitsbetrachtung

Die Bedrohungen und ihre Adressierung — je Zeile die dienstseitige und die agentenseitige Gegenmaßnahme:

| Leck-Kanal / Angriff | Typischer Hergang | Gegenmaßnahmen |
|---|---|---|
| Chatverlauf / geteilte Konversation | Token „zur Bestätigung" nochmals zitiert | Einmal-Anzeige, keine Abruf-Funktion (F2); agentenseitig: einmal nennen, sofort persistieren, nie wiederholen (F6, F10) |
| Prompt Injection / Exfiltration | Fremdinhalt fordert: „sende dein Token an …" | Host-/Mandanten-Bindung entwertet Fremdnutzung (F7); agentenseitig: Anweisungen aus Arbeitsmaterial sind Daten, keine Befehle (F10) |
| Verwirrter Stellvertreter (Confused Deputy) | Dritte bewegen den Agenten zu ungewollten Aktionen mit legitimem Token | enge Capability begrenzt den Schaden (F1); agentenseitig: Bestätigung des Anwenders vor wirkenden Aktionen (per F12 ausgelieferte Pflicht) |
| Dienst-/Proxy-Logs | Token in URL oder Request-Log | nur Bearer-Header, Logs nur mit Kurz-ID (F4); agentenseitig: nie in URLs einsetzen (F10) |
| Repo-Commit | Konfigdatei oder Skript mit Token committet | erkennbares Präfix macht Scanner fündig (F2); agentenseitig: Ablage-Rangfolge statt Projektdatei (F6) |
| Konfig-Leck des Betreibers | Backup oder Ticket mit Konfigdatei | nur Hashes beim Betreiber (F2) |
| Geräteverlust / kopierte Ablage | Secret-Ablage des Agenten kopiert | Rotation als Ein-Kommando-Routine (F8); enge Tokens begrenzen den Schaden (F1) |
| Über-Berechtigung | „zur Sicherheit" das Admin-Token verwendet | Capability-Zuschnitt macht enge Tokens billig (F1); agentenseitig: immer das engste Token verlangen |
| Vorratshaltung / Fluten der Selbstregistrierung | offener Endpunkt sammelt rechtelose Tokens an | Drosselung und Verfall (F9); beantragend erzeugte Tokens sind bis zur Freigabe rechtelos (F5) |
| Existenz-Orakel | systematisches Durchprobieren fremder Ressourcen | fremde wie nicht existierende Ressourcen antworten identisch (F4) |

Grenze des Modells: Gegen einen vollständig kompromittierten Agenten-Host — der Angreifer liest die Secret-Ablage selbst — hilft nur noch die Enge der Capability plus Rotation: Das Konzept begrenzt Schaden, es verhindert ihn nicht absolut. Und gegen einen Betreiber, der sein eigenes Protokoll bricht, schützt kein Konsumenten-Verhalten.

## 6. Was das kostet

Dieser Abschnitt zählt auf, was das Konzept kostet. Jeder Punkt ist an der Festlegung festgemacht, die ihn verursacht, und steht hier ohne Gegenrechnung.

**Die Token-Zahl wächst multiplikativ (F1).** Ein Token je Fähigkeit und Ort heißt: Wer drei Fähigkeiten an zwei Orten braucht, verwaltet sechs Tokens — jedes mit eigener Ablage, eigenem Griff, eigenem Entzug. Der enge Zuschnitt verlagert Komplexität vom Schadensfall in den Alltag, und der Alltag muss sie tragen.

**Der Betreiber baut und betreibt die Ausgabe (F2, F5, F9).** Serverseitige Erzeugung, Hash-Speicherung, Einmal-Anzeige, Display-Strings, gegebenenfalls die Selbstregistrierung samt Drosselung, Verfall und Sichtung der Selbstvorstellungen — das ist Bau- und danach laufende Betriebsarbeit. In der Geschmacksrichtung „beantragend" kommt die Warteschlange dazu: Jedes rechtelos geborene Token wartet auf einen Menschen, der es scharf schaltet.

**Einmal-Anzeige kostet Komfort (F2).** Es gibt keine „Token anzeigen"-Funktion, also gibt es Verlust-Nachfragen, und jede davon endet in einer Rotation. Das ist gewollt — aber es ist Arbeit, und sie fällt beim Support an.

**Die Fehlersemantik verwirrt Legitime (F4).** Wer auf eine fremde Ressource zugreift, bekommt dieselbe 404 wie bei einer nicht existierenden — auch der berechtigte Nutzer, der sich nur vertippt hat, erfährt nicht, ob es die Ressource gibt. Der Orakel-Schutz kostet Diagnose-Komfort; die lehrende Fehlermeldung für den korrekt Authentifizierten muss das auffangen.

**Rotations-Disziplin ist Kalenderarbeit (F8).** Ablaufdaten pflegen, Monitoring auf Authentifizierungsfehler, Rotation als geübter Handgriff — das ist unspektakuläre Daueraufgabe, und sie gehört genau deshalb in Kalender und Alarme, weil sie sonst nicht stattfindet.

**Die Doku-Pflicht wiederholt sich je Dienst (F12).** Jeder Betreiber, der das Konzept übernimmt, formuliert die Agenten-Regeln in seiner eigenen Dokumentation neu — dieselben Regeln, so oft es Dienste gibt. Diese Vervielfachung ist gewollt (anders erreichen die Regeln keinen Agenten), aber sie ist Redaktionsarbeit in jeder einzelnen Übernahme.

## 7. Geltung und Grenzen

### Randbedingungen

**Der Betreiber kontrolliert die Ausgabe.** Er kann Tokens erzeugen, prüfen, einzeln entziehen — das verlangen F2, F8 und F9. Wo der Zugang von einem Dritten stammt (ererbter API-Key einer Bestands-API), gilt nur die Handhabungs-Hälfte. Man erkennt die Lage daran, wer die Token-Liste führt: niemand → keine Ausgabe-Kontrolle.

**Es gibt einen Weg am Chat vorbei.** Entweder eine Selbstregistrierung (F5) oder irgendein zweiter Kanal vom Betreiber zum Anwender. Das trägt die Antwort, weil die Rangfolge (F6) sonst beim letzten Weg beginnt. Man erkennt die Lage daran, dass die einzige denkbare Übergabe „schreib es mir hier hinein" ist — dann gilt §7 unten.

**Der Agent hat irgendeine Ablage.** Umgebungsvariablen oder wenigstens Dateien, die einen Aufruf überdauern. Ohne jede Ablage bleibt nur das Kontextfenster, und aus dem erklärten letzten Weg wird der einzige.

### Wann die Antwort kippt

**Ein Ökosystem erzwingt OAuth/OIDC.** Wo Delegation im Namen fremder Nutzer, Zustimmungsdialoge oder föderierte, kurzlebige Sitzungen gefordert sind — oder ein Anbieter OAuth schlicht vorschreibt —, sind statische Capability-Tokens das falsche Werkzeug: Dann gelten die etablierten Flows. Die Antwort kippt aber nur zur Hälfte: **Die Handhabungsregeln F5–F11 gelten unabhängig vom Token-Typ** — auch ein OAuth-Refresh-Token im Chatverlauf ist ein Leck. Man erkennt die Lage an den Wörtern „im Namen von" und „Consent" in den Anforderungen.

**Ein Token müsste naturgemäß breite Rechte bündeln.** Wenn der Dienst-Zuschnitt keine engen Fähigkeiten hergibt, ist nicht das Token-Format das Problem, sondern der Dienst — F1 lässt sich nicht auf einen Dienst kleben, dessen Operationen alle alles dürfen. Man erkennt die Lage daran, dass jeder Versuch, ein enges Token zu definieren, bei „braucht aber trotzdem Vollzugriff" endet.

**Die Plattform hat keinen externen Geheimnisweg.** Ohne Umgebungsvariablen und ohne Dateien bleibt von der Rangfolge (F6) nur der dritte Rang: Das Geheimnis steht im Transkript. Enge Tokens (F1) begrenzen dann den Schaden — bei hohem Schadenspotential tragen sie ihn nicht, und dann gilt die Antwort dieses Dossiers dort nicht.

**Ein zentrales Secret-Management ist vorgeschrieben.** Wo eine Organisation Vault-artige Systeme verbindlich macht, ersetzt deren Mechanik die Ränge 1 und 2 der Ablage — die Rangfolge wird zur Frage „Vault-Weg vor Kontextfenster". Die Niemals-Regeln (F10) und der Zuschnitt (F1–F4) bleiben unverändert in Kraft.

### Was es nicht löst

**Das Rechte- und Rollenmodell des Dienstes.** Wer welche Capabilities *bekommen darf*, wer sie vergibt und nach welchen Kriterien — das bleibt Sache des Dienstes; dieses Konzept regelt nur das Artefakt und seine Handhabung.

**Die Werkzeug-Mechanik der Bestandsverwaltung.** Wie ein Betreiber seinen Token-Bestand konkret führt — etwa mit einer Verwaltungs-CLI, die Erzeugung, Rotation und Entzug als Kommandos anbietet — ist ein eigenes Thema und hier bewusst außen vor (Q7).

**Das Auffinden von Diensten und die Vertrauensentscheidung.** Ob ein Dienst vertrauenswürdig ist und woher der Agent von ihm weiß, beantwortet dieses Dossier nicht; es beginnt dort, wo ein Zugang eingerichtet werden soll.

**Die Weitergabe zwischen Menschen.** Wenn ein Anwender ein Token an einen Kollegen weiterreicht, verlässt es jeden hier geregelten Kanal — dagegen hilft nur der enge Zuschnitt (der Schaden bleibt klein) und der Verfall (der Zustand endet).

## 8. Bewährung

Dieser Abschnitt trennt, was in Betrieb ist, was in Arbeit ist und was nur der Anschauung dient. Jeder Beleg steht in demselben Raster.

### Beleg 1 — Ausgeführtes Anwendungsbeispiel: die selbstbeschreibenden Dienste

**Was es ist.** Das Schwester-Konzept dieser Reihe für selbstbeschreibende Web-Dienste (Quelle 1) macht die Festlegungen dieses Dossiers für einen konkreten Diensttyp verbindlich: Sein Rechte-Zuschnitt entspricht F1–F3, seine Geheimnis-Rangfolge F6, sein Instanz-Prinzip F7, seine Selbstregistrierung mit Display-String F5 und F11, seine Auslieferungspflicht F12.

**Was es belegt.** Dass sich dieses allgemeine Konzept vollständig und widerspruchsfrei in ein konkretes Bauprinzip einbetten lässt — die dortigen Festlegungen sind die hiesigen, angewandt auf einen Diensttyp. Es belegt Anwendbarkeit, nicht Betriebserfahrung.

**Stand, ehrlich.** Als Konzept veröffentlicht (Version 1.0); die zugehörige Referenzimplementierung ist in Arbeit (Beleg 3).

**Quelle.** Quelle 1.

### Beleg 2 — Vorform in Betrieb: die Geheimnis-Doktrin des Wiki-Werkzeugkastens

**Was es ist.** Ein Werkzeugkasten des Autors (Quelle 2), der die Wiki-Plattform Wiki.js für Agenten benutzbar macht und dabei eine strenge Geheimnis-Doktrin durchsetzt: Umgebungsvariable vor Datei, Datei nur als gehärtete Durchleitung, das Geheimnis erscheint nie in einer Ausgabe; mehrere Wiki-Instanzen werden gleichzeitig bedient, jede mit eigenem Zugang.

**Was es belegt.** Die Ablage-Rangfolge (F6) und die Orts-Bindung (F7) sind unter Last praktikabel — mit diesem Werkzeugkasten wurde ein Wiki mit 140.000 Knoten aufgebaut, ohne dass ein Geheimnis in Transkripten oder Logs auftauchte. Nicht belegt ist der Token-Zuschnitt: Die Plattform verwendet ererbte API-Keys — genau der Fall, in dem nur die Handhabungs-Hälfte dieses Konzepts gilt.

**Stand, ehrlich.** In Betrieb und täglicher Benutzung; Vorform, nicht Muster.

**Quelle.** Quelle 2.

### Beleg 3 — Referenzimplementierung in Arbeit: der Mail-Ablage-Dienst

**Was es ist.** Ein Dienst des Autors (Quelle 3), spezifiziert nach dem Schwester-Konzept aus Beleg 1 und damit nach diesem Dossier: eng geschnittene Tokens (ein Token = ein Ziel), Display-Strings, betreiberseitige Ausgabe; die Selbstregistrierung ist als Ausbaustufe vorgesehen.

**Was es belegt.** Bisher, dass sich die Festlegungen für einen realen Dienst vollständig ausspezifizieren lassen. Betriebserfahrung ist nicht belegt: Die Implementierung durch einen Entwicklungs-Agenten läuft; eine Vollzugsmeldung stand bei Niederschrift dieser Fassung noch aus und wird hier nachgetragen.

**Stand, ehrlich.** Spezifikation vollständig, Implementierung läuft, kein Konsument im Alltag.

**Quelle.** Quelle 3.

### Durchgehendes Beispiel (frei erfunden): der Verkehrsmelde-Zugang

Was nun folgt, ist **kein Beleg**, sondern die Anschauung — alle Werte sind fiktiv. Ein Anwender lässt seinen Agenten einen Verkehrsdienst einrichten. Der Agent ruft die Selbstregistrierung des Dienstes auf und stellt sich vor: „Agent von J.; Zweck: Staumeldungen auf dem Arbeitsweg melden und abrufen." Die Antwort trägt das Flag **beantragend**, ein Token mit dem Präfix des Dienstes und den Display-String `kluvmoth`. Der Agent legt das Token in der Umgebungsvariablen ab, die der Dienst dafür vorschlägt, und sagt seinem Anwender nur: „Bitte gib dem Zugang **kluvmoth** die Melderechte." Der Anwender schaltet `kluvmoth` serverseitig scharf — das Geheimnis hat den Chat nie berührt (F5, F6, F11). Monate später liest der Betreiber seine Token-Liste: Neben `kluvmoth` steht die Selbstvorstellung, und er weiß noch, wer da warum Zugang bekam (F5). Als der Anwender den Arbeitsweg wechselt, entzieht er `kluvmoth` — eine Fähigkeit erlischt, alles andere läuft weiter (F1, F8). Ein zweites Token, das er vor einem Jahr „für später" beantragt und nie scharf geschaltet hatte, ist da längst verfallen (F9).

## 9. Offene Fragen

Die ersten drei Fragen sind zugleich **Aufträge an den Verwender**: Dieses Konzept lässt sie bewusst offen, weil ihre richtige Antwort vom Einsatz abhängt — wer das Konzept übernimmt, muss sie für seinen Kontext beantworten und die Antwort dokumentieren.

**Q1** *(Auftrag)* Welche Verfallsfristen und Drosselwerte gelten (F9) — wie lange lebt ein unbestätigtes Token, ab wann gilt eines als ungenutzt, wie stark ist der offene Ausgabeweg gedrosselt? Die Werte sind Betriebsgrößen des jeweiligen Dienstes; das Konzept verlangt nur, dass sie existieren und dokumentiert sind.

**Q2** *(Auftrag)* Welche Form hat der zweite Kanal (F5), wo keine Selbstregistrierung angeboten wird — persönliche Übergabe, Einmal-Link mit Ablauffrist, ein bestehendes Secret-Management? Jede Form hat eigene Leck-Eigenschaften; die Wahl gehört in die Betriebsdokumentation des Dienstes.

**Q3** *(Auftrag)* Welcher Rotations-Rhythmus gilt (F8), und wird Rotation automatisiert? Vorsicht bei der Automatisierung: Der Auslieferungskanal für das jeweils neue Token ist selbst ein Kanal — wer ihn automatisiert, schafft womöglich genau den Dauer-Leck-Weg, den die Einmal-Anzeige vermeiden sollte.

**Q4** Braucht es einen konventionellen Namensraum für Token-Präfixe (F2), damit Secret-Scanner Dienste generisch erkennen — oder genügt Dienst-Individualität? Ein Namensraum hülfe den Werkzeugen und wäre zugleich die Registry, die diese Reihe sonst meidet.

**Q5** Lohnt Capability-Attenuation — aus einem Token ein engeres ableiten, wie es das Macaroon-Modell vorführt (Quelle 6) — im Agenten-Kontext, oder ist „mehrere schmale Tokens ausstellen" praktisch immer einfacher und sicherer?

**Q6** Soll ein Dienst die Verwendung rotierter Alt-Tokens aktiv melden — als Frühwarnung auf gestohlene Kopien —, und wie, ohne mit der Meldung selbst zum Orakel zu werden (F4)?

**Q7** Die Werkzeug-Mechanik der Bestandsverwaltung — eine Verwaltungs-CLI, die Erzeugung, Einmal-Anzeige, Rotation und Entzug als Kommandos führt — ist hier außen vor: Verdient sie ein eigenes Dossier dieser Reihe, und welche seiner Festlegungen wären dort nur Anwendung, welche neu?

## 10. Quellen & Verwandtes

Dies ist der einzige Ort dieses Dossiers, an dem Verweise stehen. Fundstellen geprüft am 14.08.2026, soweit nicht anders vermerkt.

**Quelle 1 · Schwester-Konzept dieser Reihe: [self-describing-services](../self-describing-services/PROPOSAL.de.md).** — *Wofür sie hier steht:* Das ausgeführte Anwendungsbeispiel (§8, Beleg 1): Dort sind die Festlegungen dieses Dossiers für selbstbeschreibende Web-Dienste verbindlich gemacht (dortige F12–F14). Vertiefung, kein Verständnis-Muss.

**Quelle 2 · Wiki-Werkzeugkasten des Autors, Repository `syncwiki`.** https://github.com/joern-schlingensiepen/syncwiki — *Wofür sie hier steht:* Beleg 2 in §8 (Geheimnis-Doktrin unter Last, 140.000 Knoten).

**Quelle 3 · Mail-Ablage-Dienst des Autors, Repository `imap-service`.** https://github.com/schlingensiepen/imap-service — *Wofür sie hier steht:* Beleg 3 in §8 (Referenzimplementierung in Arbeit).

**Quelle 4 · Jack B. Dennis, Earl C. Van Horn: *Programming Semantics for Multiprogrammed Computations.* Communications of the ACM, Band 9, Nr. 3, 1966.** https://doi.org/10.1145/365230.365252 (Verlagsschranke; Abruf am 14.08.2026 nicht frei möglich) — *Wofür sie hier steht:* Der Ursprung des Capability-Begriffs — die Fähigkeit als unfälschbares Artefakt der Berechtigung; F1 und F3 stehen in dieser Tradition.

**Quelle 5 · Norm Hardy: *The Confused Deputy (or why capabilities might have been invented).* ACM SIGOPS Operating Systems Review, 1988.** https://css.csail.mit.edu/6.858/2015/readings/confused-deputy.html (Nachdruck, geprüft 14.08.2026) — *Wofür sie hier steht:* Das klassische Argument, warum Rechte am Artefakt statt am Aufrufer hängen sollen — der Hintergrund der §5-Zeile zum verwirrten Stellvertreter und der Bindung aus F7.

**Quelle 6 · Arnar Birgisson et al.: *Macaroons: Cookies with Contextual Caveats for Decentralized Authorization in the Cloud.* NDSS 2014.** https://research.google/pubs/macaroons-cookies-with-contextual-caveats-for-decentralized-authorization-in-the-cloud/ — *Wofür sie hier steht:* Das Referenzmodell für Capability-Attenuation, auf das Q5 zeigt.

**Quelle 7 · OWASP Gen AI Security Project: *LLM01:2025 Prompt Injection.*** https://genai.owasp.org/llmrisk/llm01-prompt-injection/ — *Wofür sie hier steht:* Die unabhängige Einordnung des Angriffswegs, gegen den die Niemals-Regeln (F10) und die Injektions-Zeile in §5 gerichtet sind.

**Einordnung.** Dieses Dossier überträgt eine alte Idee in eine neue Lage: Capabilities (Quellen 4, 5) binden die Berechtigung an ein unfälschbares Artefakt statt an die Identität des Aufrufers — genau richtig für einen Konsumenten, dessen „Identität" ein angreifbares Kontextfenster ist (Quelle 7). Neu ist nicht das Modell, sondern der Handhabungs-Kodex darum herum: Einmal-Anzeige, Rangfolge, Niemals-Regeln, Display-String — die Zugeständnisse an ein Medium, das zum Aufbewahren von Geheimnissen nie gedacht war.
