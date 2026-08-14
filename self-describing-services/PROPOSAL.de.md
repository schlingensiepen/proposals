---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: self-describing-services
language: de (führend)
question: "Wie mache ich als Anwender meinen Agenten einen Dienst verfügbar — ohne Installation, auf jeder Plattform, in jeder Sandbox?"
thesis: "Ein Dienst, der sich selbst beschreibt, macht Installation überflüssig: Der Agent merkt sich nur die klassische Tool-Beschreibung — wenn du das willst, schau unter dieser URL — und ein Token; alles Weitere liefert der Dienst zur Laufzeit selbst, und weil es keine zweite Quelle gibt, veraltet nichts."
---

# Selbstbeschreibende Dienste

**In einem Satz:** Ein Dienst, der sich selbst beschreibt, macht Installation überflüssig — der Agent merkt sich nur die klassische Tool-Beschreibung („wenn du das willst, schau unter dieser URL") und ein Token; alles Weitere liefert der Dienst zur Laufzeit selbst, und weil es keine zweite Quelle gibt, veraltet nichts.

**Kurzthese:** Einem Agenten einen Dienst zu geben ist heute ein Einrichtungsprojekt: Connector installieren, Skill verteilen, Rechte klicken — und jede installierte Beschreibung beginnt sofort zu veralten. Ein selbstbeschreibender Dienst kollabiert das Onboarding auf einen Merksatz — „wenn du das willst, schau unter dieser URL": die klassische Tool-Beschreibung, ein Zweck und eine Adresse. Alles Weitere — Einstieg, Benutzungsbeschreibung, Schnittstellenbeschreibung — holt der Agent aktuell vom Dienst selbst; die gestaffelte Auslieferung ist genau dafür da, dass dieser Merksatz genügt. Das funktioniert in jeder Sandbox, denn es verlangt nur zwei Fähigkeiten, die jeder Agent hat: HTTP und Merken.

## 1. Problem

Der Autor hat sich diese Frage als **Anwender** gestellt: Da ist ein Dienst — ein Postfach-Ablagedienst, ein Wiki, eine Fach-API — und da sind Agenten auf verschiedenen Plattformen, in Cloud-Sandboxen, im Terminal, im Browser. Warum ist „benutz das bitte" ein Einrichtungsprojekt? Für jede Plattform ein Connector, für jeden Connector eine Installation, für jede Installation Berechtigungsdialoge — und am Ende weiß der Agent trotzdem nur das, was ihm jemand vor Wochen aufgeschrieben hat.

Vier Rollen tragen das Problem gemeinsam:

**Der Anwender** will sagen können „nimm diesen Dienst dazu" — als Satz, nicht als Projekt. Er hat mehrere Agenten auf mehreren Plattformen, und viele davon laufen in Sandboxen, in denen sich schlicht nichts installieren lässt. Was ein Agent dort kann, ist: HTTP sprechen und sich etwas merken. Ein Onboarding, das mehr verlangt, schließt genau die Umgebungen aus, in denen Agenten zunehmend arbeiten.

**Der Betreiber** eines kleinen oder internen Dienstes kann nicht für jede Agentenplattform einen Connector bauen, veröffentlichen und aktuell halten. Jede verteilte Beschreibung seines Dienstes — Skill-Datei, Connector, Wiki-Seite — ist eine Kopie, die er nie wieder vollständig einfängt: Das ist ein N-Dienste-mal-M-Plattformen-Problem, und es wächst mit jedem Release.

**Der Nachrüster** steht vor einer fremden Bestands-API, die er nicht besitzt und nicht ändern kann — ein Wiki, ein Ticketsystem, eine Fach-API —, und will sie seinen Agenten verwendbar machen. Ihm bleibt heute nur die verteilte Skill-Datei: eine Kopie fremden Wissens, die mit jedem Update des Zieldienstes leiser falsch wird und die er an jeden Konsumenten einzeln ausliefern muss.

**Der Agent** schließlich glaubt seiner Anleitung wörtlich. Ein menschlicher Entwickler wird stutzig, wenn die Doku nicht zur API passt, und probiert; ein Agent produziert aus einer veralteten Beschreibung perfekt formatierte, falsche Aufrufe — mit Selbstvertrauen. Für ihn ist eine Anleitung, die veraltet sein *kann*, eine Anleitung, die irgendwann *lügt*.

Der übliche Ausweg — mehr Disziplin bei der Doku-Pflege — bekämpft das Symptom. Die strukturelle Antwort setzt woanders an: Installation und Kopie sind das Problem. Was nicht verteilt wird, muss nicht installiert werden; was nicht separat existiert, kann nicht separat veralten.

## 2. Das Konzept

### 2.1 Die Ausgangslage: Schnittstellen beschreiben sich fast schon selbst

Eine Web-Schnittstelle tut zunächst etwas sehr Gewöhnliches: Sie bietet Operationen an — Daten lesen, Daten schreiben, Dateien entgegennehmen — und genau diese **Mechanik** wird heute in weiten Teilen automatisch dokumentiert. Moderne Frameworks erzeugen die Schnittstellenbeschreibung direkt aus dem Code: Bei HTTP-APIs ist **OpenAPI** die etablierte Form — Routen, Felder, Typen, Statuscodes fallen beim Bauen ab, ohne gepflegt zu werden, und liegen maschinenlesbar unter einer URL des Dienstes selbst. Das ist eine hervorragende Ausgangslage: Der Vertrag existiert schon, er ist schon generiert, er ist schon abrufbar. Man muss ihn nur noch **agentenfreundlich anpassen** — „aufpumpen": jedes Feld beschrieben, jedes Beispiel echt, jeder Fehler erklärt —, und aus der Formalie wird eine Funktion.

### 2.2 Die Parallele: Was MCP für Tools ist, wird die Web-API für Dienste

Das Model Context Protocol hat vorgemacht, wie Agenten-Integration ohne Wissens-Verteilung aussieht: Ein MCP-Server beschreibt seine vielen Methoden **zur Laufzeit**, der Client fragt sie ab — Discovery statt Distribution. Dieses Konzept überträgt genau dieses Prinzip auf die Web-API. Der entscheidende Unterschied: Ein MCP-Server muss beim Konsumenten erst *angebunden* werden — Eintrag, Prozess, Berechtigung. Den Web-Client dagegen hat jeder Agent längst eingebaut; HTTP ist die eine Fähigkeit, die überall vorhanden ist, bis in die abgeriegelte Sandbox. Wir machen also nichts anderes, als die Selbstbeschreibung eines MCP auf die Schnittstelle zu verlegen, die keine Anbindung braucht.

### 2.3 Zwei Beschreibungsarten — und ein bewusster Bruch

Das Wissen über einen Dienst zerfällt in zwei Arten. Die **Schnittstellenbeschreibung** trägt die Mechanik: was es gibt und wie man es korrekt aufruft. Die **Benutzungsbeschreibung** trägt das übergreifende Verhalten: wofür der Dienst gedacht ist, in welcher Reihenfolge man arbeitet, welche Pflichten gegenüber dem Anwender gelten, wie mit dem Zugang umzugehen ist. Die saubere Trennung ist aber nur der Anfang — wir **durchbrechen sie mit Absicht**: Beim Aufpumpen wandert alles Verhalten, das an einer einzelnen Operation klebt („dieser Aufruf ist asynchron", „das braucht Bestätigung deines Anwenders", „dieser Fehler bedeutet: erst hochladen"), *in die Schnittstellenbeschreibung hinein*, direkt an die Operation. Der Gewinn ist doppelt: Der Agent findet die Regel genau dort, wo er den Aufruf baut — und die Benutzungsbeschreibung bleibt klein, weil sie nur noch das wirklich Übergreifende trägt.

### 2.4 Der Einstieg: Hypertext, zur Laufzeit gerendert

Alles beginnt an einer einzigen Adresse: Der **Einstieg** an der Basis-URL ist eine unauthentifizierte Kurzbeschreibung von ~20 Zeilen — *und* das Verzeichnis von allem Weiteren. Er sagt, was der Dienst ist und wie man Zugang bekommt, und er **verlinkt jeden Teil der Beschreibung**: die Benutzungsbeschreibung, die Schnittstellenbeschreibung, die thematischen Teil-Beschreibungen (jeweils mit einem Satz, was der Teil leistet), den Anker-Skill. Das ist die alte Stärke von Hypertext, angewandt auf Selbstauskunft: Man muss nichts wissen außer einer Adresse — von dort ist alles erreichbar, und an jedem Link steht, was einen dahinter erwartet. Wer so einen Einstieg baut, setzt die URLs **zur Laufzeit** ein, mit dem Host, unter dem der Dienst gerade angesprochen wird — jedes Beispiel ist damit für genau diese Installation kopierfähig. Die Folge „Kurzbeschreibung – Link – lange Beschreibung – Link – …" ist dabei kein Stilmittel, sondern **Kontextfenster-Schonung**: Geladen wird an jeder Stufe nur, was die nächste Entscheidung braucht — der Rest bleibt Link. Und Hypertext ist hier als **Konzept** gemeint, nicht als Format: Die Stufen sind in der Regel Markdown mit Links — für Agenten wie Menschen unmittelbar lesbar; HTML braucht es dafür nicht.

### 2.5 Ein Durchlauf: der Verkehrsdienst

Wie sich das anfühlt, zeigt ein erfundener Dienst, der uns durch das ganze Dossier begleitet *(frei erfunden und didaktisch zugeschnitten; die reale Bewährung steht in §8)*: ein Dienst, der **Verkehrsdaten sammelt und bereitstellt**. Sein Einstieg gliedert die Schnittstelle in vier erklärte Teile: **Verkehrslage erfahren** (der häufige Lesefall) · **Meldungen beitragen** (der Schreibfall) · **Datensätze hochladen** in verschiedenen Formaten (selten gebraucht, umfangreich) · **POI ablegen und abrufen** (statische Orte von Interesse — nützlich, aber nicht in jedem Anwendungsfall). Ein Agent kommt nie „einfach so", er kommt mit einem Anliegen: Er will etwas *erfahren* oder etwas *beitragen*. Am Einstieg nimmt er die Weiche und lädt **nur** die Teil-Beschreibung seines Anliegens — die Upload-Formate und die POI-Verwaltung liest er gar nicht erst und verstopft sich sein Kontextfenster nicht damit. Aber weil er die Kurzfassung kennt, weiß er für immer, *wo* er suchen muss, wenn sein Anwender nächste Woche doch POI pflegen will.

Und noch etwas zeigt der Durchlauf: **Ein Dienst kann mehrfach im Leben eines Anwenders vorkommen.** Der Verkehrsdienst der eigenen Stadt und der des Urlaubsorts, das Firmen-Wiki und das private — gleiche Software, gleiche API, aber je eigene Basis-URL, eigene Rechte, eigenes Geheimnis. Gemerkt wird deshalb immer die **Instanz**, nie „die Software"; was das im Einzelnen heißt, regelt F9.

### 2.6 Bauanleitung — und Nachrüstung gleich mit

Die Festlegungen in §4 sind eine **Bauanleitung**: Sie sagen, wie man eine solche selbstbeschreibende Schnittstelle *schreibt*. Und sie gelten von Anfang an doppelt. Entweder man baut einen **eigenen Dienst** und liefert die Beschreibung aus ihm selbst aus — oder man rüstet eine **fremde Bestands-API nach**: Ein eigener kleiner Beschreibungsdienst, die **Fassade**, liefert die komplette Kaskade für die fremde API aus und sagt dem Agenten „führe die hier beschriebenen Operationen direkt gegen den Originaldienst aus". Die Fassade berührt keinen Datenverkehr; sie macht eine API, die sich nicht selbst beschreibt, *sofort verwendbar*. Bau und Nachrüstung sind dieselbe Anleitung mit anderem Auslieferungsort — deshalb steht die Fassade hier am Anfang als Anwendung und am Ende (F15) als Festlegung.

### 2.7 Zugang: In jedem Fall ein Geheimnis — manchmal ist es ein Token

Bleibt der Zugang. Bei einem **Neubau** empfehlen wir das Token-Modell: eng geführte Tokens, jedes verkörpert genau eine Fähigkeit (F12). Bei einer **Bestands-API** bringt die Authentifizierung mit, was sie hat — einen API-Key, ein Passwort. So oder so gilt: **In jedem Fall hat man ein Geheimnis; manchmal ist es ein Token.** Und für jedes Geheimnis, gleich welcher Herkunft, gilt dieselbe Handhabungs-Rangfolge — Umgebungsvariable vor Durchleitungs-Datei vor Kontextfenster; im Einzelnen regelt das F13.

### 2.8 Die Transmissionskette: Wer dieses Dossier liest — und wer nicht

Ein konsumierender Agent kennt dieses Dossier nicht, und er muss es nicht kennen: Er kennt nur die Selbstbeschreibung des Dienstes, und die ist in sich geschlossen. Deshalb ist jede Regel dieses Dossiers, die sich an Konsumenten richtet, in Wahrheit eine **Auslieferungspflicht des Erbauers**: Sie muss in der Benutzungsbeschreibung des Dienstes selbst stehen, in dessen eigenen Worten. Die Kette lautet *Dossier → Erbauer → Selbstbeschreibung → Agent* — dieses Dokument instruiert den, der baut; der Dienst instruiert den, der benutzt (F11).

## 3. Begriffe

**Schnittstellenbeschreibung** — die maschinenlesbare Beschreibung der Mechanik: Operationen, Felder, Typen, Fehlerformate, Beispiele; aus dem Code generiert und redaktionell angereichert (bei HTTP-APIs typisch OpenAPI).

**Benutzungsbeschreibung** — das vom Dienst ausgelieferte Prosa-Dokument mit dem übergreifenden Verhalten: wofür der Dienst gedacht ist, Arbeitsablauf, Pflichten gegenüber dem Anwender, Umgang mit dem Geheimnis.

**Einstieg** — die unauthentifizierte Kurzbeschreibung (~20 Zeilen) an der Basis-URL; verzeichnet und verlinkt alle übrigen Teile der Beschreibung samt deren Zweck.

**Kaskade** — die Staffelung Einstieg → Benutzungsbeschreibung → Schnittstellenbeschreibung (samt Teil-Beschreibungen), geordnet nach Ladekosten: erst entscheiden, dann laden.

**Teil-Beschreibung** — der thematische Ausschnitt der Beschreibung für einen abgegrenzten Aufgabenbereich (beim Verkehrsdienst: erfahren · beitragen · Datensatz-Upload · POI); einzeln ladbar, am Einstieg mit Zweck verzeichnet.

**Anker-Skill** — der vom Dienst bereitgestellte, speicherbare Skill, der nichts enthält als Zweck, Ziel und Links auf die Selbstbeschreibung: eine leere Hülse, die im Skill-Katalog des Anwenders das Auslösen verankert. *Nicht zu verwechseln mit der Fassade:* Der Anker-Skill leitet zum Dienst weiter, die Fassade beschreibt eine fremde API.

**Fassade** — ein eigener Beschreibungsdienst, der die Kaskade für eine fremde Bestands-API ausliefert, ohne deren Datenverkehr zu berühren (Nachrüstung).

**Mandant** — ein abgegrenzter Nutzungsbereich eines mehrmandantenfähigen Dienstes (eigene Kennungen, eigene Rechte, ggf. eigener Hostname); Discovery-Entscheidungen können je Mandant getroffen werden.

**Instanz** — eine konkrete Installation eines Dienstes bzw. einer Dienstsoftware unter eigener Basis-URL. Dieselbe Software kann im Leben eines Anwenders mehrfach vorkommen (das Firmen-Wiki, das private Wiki); gemerkt, verankert und mit Rechten versehen wird je Instanz.

**Harness** — die Laufzeitumgebung, in der ein Agent läuft, mitsamt ihren Ablagemöglichkeiten: Session-Gedächtnis, Skill-Katalog, Umgebungsvariablen. Der Harness gehört dem Anwender bzw. seiner Plattform, nicht dem Dienst.

**Geheimnis** — jedes vertrauliche Zugangsdatum, das der Konsument zum Aufruf braucht: API-Key, Passwort — oder ein Token.

**Capability** — eine einzelne, eng abgegrenzte Fähigkeit an einem Dienst (etwa: Meldungen beitragen). Der Rechte-Zuschnitt aus F12 denkt in Capabilities; das Token ist das Artefakt, das genau eine davon verkörpert.

**Token** — das empfohlene Zugangsartefakt bei Neubauten: eng geschnitten, eine Fähigkeit pro Token. *Jedes Token ist ein Geheimnis; nicht jedes Geheimnis ist ein Token.*

**Selbstregistrierung** — die optionale offene Methode, über die ein Konsument gegen eine Freitext-Selbstvorstellung ein Token erhält; in zwei Geschmacksrichtungen: automatisch und beantragend (→ F14).

**Display-String** — der kurze, aussprechbare, **nicht geheime** Griff zu einem Token (8 Zeichen, Kleinbuchstaben), über den Menschen über Tokens sprechen und Rechte vergeben, ohne je das Geheimnis zu nennen (→ F14).

## 4. Festlegungen

**Landkarte.** Fünfzehn Festlegungen in vier Blöcken. **F1–F7** richten sich an den *Erbauer* — sie sagen, wie die Beschreibung entsteht und ausgeliefert wird; bei einer Nachrüstung treffen sie den Fassaden-Betreiber. **F8–F11** regeln die *Konsumentenseite*: wie ein Dienst angeboten, je Instanz gemerkt und verfügbar gemacht wird — wobei F11 die Brücke ist: Konsumenten-Regeln erreichen den Agenten ausschließlich über die Selbstbeschreibung, die der Erbauer ausliefert (§2.8). **F12–F14** sind das Zugangs-Geschirr — Token-Empfehlung, Geheimnis-Handhabung, Selbstregistrierung — und tragen unabhängig: Sie gelten auch dort, wo sonst nichts von diesem Konzept übernommen wird. **F15** dehnt das Ganze auf fremde Dienste aus. F1 trägt alles: Fällt „eine Quelle", zerfällt der Rest zu gewöhnlichen Doku-Ratschlägen. Jede Festlegung nennt, für wen sie gilt; das ausgeführte Beispiel zum ganzen Ablauf steht in §8.

### F1 · Eine Quelle: Der Dienst beschreibt sich selbst

**Gilt für:** Erbauer (bei Nachrüstung: Fassaden-Betreiber).

**Was gilt.** Die maßgebliche Beschreibung eines Dienstes — Einstieg, Benutzungsbeschreibung, Schnittstellenbeschreibung — wird vom Dienst selbst zur Laufzeit ausgeliefert; eine getrennt verteilte und gepflegte Beschreibung gibt es nicht.

**Warum.** Jede Kopie beginnt im Moment ihrer Erstellung zu veralten, und Agenten glauben Anleitungen wörtlich: Aus einer veralteten Beschreibung entstehen perfekt formatierte falsche Aufrufe. Zugleich entfällt mit der Verteilung die Installation — und damit das N×M-Problem aus Diensten und Plattformen.

**Naheliegend und verworfen.** *Skill-Dateien verteilen* — veralten und müssen zu jedem Konsumenten gebracht werden; genau das Ausgangsproblem. *Ein Konnektor-/Vermittlerdienst je Plattform* — eine zweite Implementierung der API, die installiert werden muss und eigenständig driftet. *Doku-Disziplin und Release-Checklisten* — bekämpfen das Symptom; strukturell bleibt die zweite Quelle bestehen.

**Woran man merkt, dass es verletzt ist.** Zwei Beschreibungen desselben Dienstes widersprechen sich in einem Detail. Der Frühindikator ist klein: schon eine Operationszählung, die in einem Artefakt „12" und im Nachbardokument „13" lautet — ein real beobachteter Fall, aufgelöst in §8.

### F2 · Einstieg und Kaskade: Alles verlinkt, erst entscheiden, dann laden

**Gilt für:** Erbauer.

**Was gilt.** Der unauthentifizierte Einstieg (~20 Zeilen an der Basis-URL) genügt für die Entscheidung, ob der Dienst gebraucht wird — und er **verzeichnet und verlinkt jeden Teil der Beschreibung**: Benutzungsbeschreibung, Schnittstellenbeschreibung, jede Teil-Beschreibung (je mit einem Satz Zweck) und den Anker-Skill. Die Stufen sind untereinander per Link verknüpft; nachgeladen wird nur, was tatsächlich gebraucht wird, und erst, wenn die Entscheidung gefallen ist — erreichbar sind alle Beschreibungsstufen ohne Autorisierung (F4).

**Warum.** Das Kontextfenster ist das knappe Gut des Konsumenten: Ein Agent muss einen Dienst ablehnen können, *bevor* er ihn vollständig gelesen hat. Und die Voll-Verlinkung ist die Hypertext-Stärke, die das Merken minimal macht — wer eine Adresse kennt, erreicht von dort alles und weiß an jedem Link, was ihn dahinter erwartet.

**Naheliegend und verworfen.** *Ein einziges großes Beschreibungsdokument* — zwingt zur Volltextladung vor der Entscheidung. *Feste, standardisierte Pfade als Voraussetzung* — Wiedererkennbarkeit ist angenehm, aber der Einstieg erschließt den Rest per Verweis; eine Pfad-Konvention ist Komfort, keine Bedingung (bewusst offen: Q1).

**Woran man merkt, dass es verletzt ist.** Agenten laden regelmäßig die vollständige Beschreibung und stellen erst danach fest, dass der Dienst nicht passt — oder sie finden einen Beschreibungsteil nur, weil ihn jemand außerhalb des Dienstes verlinkt hat.

### F3 · Thematische Unterteilung mit erklärter Gliederung

**Gilt für:** Erbauer.

**Was gilt.** Die Beschreibung SOLL in thematische Teile zerlegt sein, deren Gliederung der Einstieg selbst erklärt: An jedem Link zu einem Teil steht, was der Teil leistet. Die Achsen wählt der Erbauer nach Anliegen der Konsumenten — etwa Kernfunktion vs. Administration, Datenzugriff vs. Rechtevergabe, Dateihandling als eigene Gruppe, weil nicht jeder Anwendungsfall Dateien umfasst. Dieselbe Unterteilung gilt für das Paket des Anker-Skills: eine Datei je Thema.

**Warum.** Ein Agent kommt mit einem Anliegen, nicht mit Lesehunger: Beim Verkehrsdienst lädt er „beitragen" und lässt Upload-Formate und POI-Verwaltung ungelesen — das Kontextfenster bleibt frei, und weil die Kurzfassung die Gliederung erklärt, weiß er dauerhaft, wo das Ungelesene wohnt. Unerklärte Gliederung dagegen zwingt zum Stochern: Dann lädt er doch alles.

**Naheliegend und verworfen.** *Alles in einem Stück erklären* — bestraft jeden Konsumenten mit der vollen Ladung, auch für die kleinste Frage. *Unterteilung nach internen Modulen statt nach Anliegen* — der Konsument denkt in „ich will etwas erfahren/beitragen", nicht in Modulnamen des Betreibers.

**Woran man merkt, dass es verletzt ist.** Agenten laden für eine einfache Abfrage regelmäßig die Voll-Beschreibung — oder die Fragen der Konsumenten verteilen sich quer zu den angebotenen Teilen, weil die Achsen an den Anliegen vorbeigehen.

### F4 · Discovery ohne Authentifizierung, ohne Geheimnisse

**Gilt für:** Erbauer.

**Was gilt.** Einstieg, Benutzungs- und Schnittstellenbeschreibung sind ohne Zugangsdaten abrufbar und enthalten weder Geheimnisse noch Betriebsinterna — und zwar per einfachem **GET**, losgelöst davon, welchen Dialekt die API selbst spricht: Auch wer GraphQL oder JSON-RPC anbietet, liefert die Beschreibung als schlichte, verlinkbare GET-Ressourcen aus. Wo die offene Auskunft stört, ist sie je Mandant abschaltbar — als bewusste Abwägung, nicht als Voreinstellung. Wer als Erbauer dennoch einen Riegel will, DARF einen Minimalschutz vorschalten: ein Sichtschutz-Token in der URL des Einstiegs, das beim Rendern (F5) in alle weiterführenden Links einkodiert wird. Das ist bewusst schwacher Schutz — URL-Bestandteile landen in Logs und Historien, ein Geheimnis im Sinne von F13 ist so ein Token nicht —, und die Empfehlung bleibt: im einfachsten Fall komplett offen.

**Warum.** Discovery muss funktionieren, *bevor* der Konsument Zugangsdaten hat — sonst kann niemand herausfinden, wie man welche bekommt. Und was für jeden abrufbar ist, darf nichts enthalten, was nicht jeder wissen darf; diese Inhaltsregel ist einfacher zu prüfen als jede Zugriffsregel.

**Naheliegend und verworfen.** *Beschreibung hinter Login* — Henne-Ei: Der Weg zum Zugang setzt den Zugang voraus. *„Interna schadet ja nichts, sieht ja nur der Kunde"* — offene Endpunkte haben keine Kundenliste.

**Woran man merkt, dass es verletzt ist.** Onboarding-Anleitungen kursieren wieder außerhalb des Dienstes — in Wikis und Chats —, weil der Dienst selbst sie nicht hergibt; oder in der offenen Beschreibung steht etwas, das ein Betreiber beim Lesen lieber löschen würde.

### F5 · Live-Templating mit der echten Basis-URL

**Gilt für:** Erbauer.

**Was gilt.** Alle Beispiele in Einstieg, Benutzungs- und Schnittstellenbeschreibung werden mit dem Host gerendert, unter dem der Dienst gerade angesprochen wird.

**Warum.** Jedes Beispiel ist damit copy-paste-fähig für genau diese Installation. Platzhalter sind eine Fehlerquelle ausgerechnet bei dem Konsumenten, der wörtlich nimmt.

**Naheliegend und verworfen.** *`example.com`-Platzhalter mit „ersetze durch deinen Host"-Fußnote* — Agenten rufen Platzhalter tatsächlich auf. *Eine konfigurierte „offizielle" URL rendern* — hinter Proxies und bei Mandanten stimmt sie nicht mit der Anfrage überein.

**Woran man merkt, dass es verletzt ist.** In den Logs stehen Anfragen gegen `example.com`.

### F6 · Der aufgepumpte Vertrag

**Gilt für:** Erbauer.

**Was gilt.** Die generierte Schnittstellenbeschreibung wird als Produkt behandelt und redaktionell angereichert — bei OpenAPI konkret: **(1)** jedes Feld trägt eine Beschreibung, die nicht den Namen wiederholt, sondern Semantik, Einheiten, Grenzen und typische Fehler nennt, dazu ein realistisches Beispiel; **(2)** jeder Endpunkt trägt Kurz- und Langbeschreibung mit den nicht-offensichtlichen Regeln — hierhin wandert das Verhalten, das an der Operation klebt (§2.3); **(3)** die Präambel enthält die Workflow-Kurzfassung in wenigen Zeilen; **(4)** Fehler sind dokumentiert wie Erfolge, jeder Fehlertyp mit Beispiel-Antwort; **(5)** all das ist als Prüfregel erzwungen („keine Operation, kein Feld ohne Beschreibung und Beispiel" als Gate im Build). Andere Schnittstellendialekte bringen ihre eigenen Vertragsmechanismen mit — GraphQL die Introspection mit Beschreibungsfeldern, JSON-RPC das OpenRPC-Dokument, gRPC die Reflection — und werden analog angereichert; Anhänge je Technologie sind vorgesehen, falls sich zeigt, dass die Analogie im Detail ausbuchstabiert werden muss.

**Warum.** Der Vertrag existiert ohnehin (§2.1) — angereichert wird er von der Formalie zur vollständigen Mechanik-Doku, die nicht veralten kann, weil sie aus demselben Code fällt wie das Verhalten des Dienstes. Und die Kuratierung *im* Träger ist der Unterschied zur kuratierten Nebenliste: Sie wird mit ausgeliefert und altert nicht getrennt.

**Naheliegend und verworfen.** *Das Rohschema ausliefern und gut* — Vollständigkeit ohne Gewichtung; der Konsument erfährt nicht, was wichtig ist, was zusammengehört, was welche Folgen hat. *Eine getrennte kuratierte Operationsliste neben dem Schema* — der Autor hat diese Vorform selbst gebaut; sie funktioniert und beweist den Wert der Kuratierung, erzeugt aber drei nebeneinander alternde Artefakte (§8). *Beschreibungspflicht als Vorsatz statt als Gate* — erodiert mit dem ersten eiligen Release.

**Woran man merkt, dass es verletzt ist.** Die Prosa-Anleitung wächst, während die Beschreibungsfelder der Schnittstelle leer bleiben — oder ein Konsument braucht neben dem Vertrag noch ein zweites Dokument, um einen einzelnen Aufruf korrekt zu bauen.

### F7 · Lehrende Fehlermeldungen

**Gilt für:** Erbauer.

**Was gilt.** Jede Ablehnung nennt den korrekten Weg — konkret, mit dem nächsten Schritt. Beim Verkehrsdienst antwortet der Upload-Endpunkt auf ein eingebettetes Bild nicht mit „ungültige Anfrage", sondern:

```json
{
  "error": "inline_image_not_accepted",
  "message": "Images cannot be embedded in a report. Upload the image first:
              POST https://verkehr.example.org/api/v1/files (returns a file_id),
              then reference it as \"photo\": {\"file_id\": \"...\"}."
}
```

**Warum.** Der Fehlermoment ist der Moment höchster Aufmerksamkeit: Beim Agenten steht die Korrekturanweisung exakt dann im Kontextfenster, wenn sie gebraucht wird — Selbstbeschreibung am Punkt des Scheiterns. Fehlertexte sind Schnittstellenbeschreibung und verdienen dieselbe redaktionelle Sorgfalt. Es ist zugleich die Einlösung einer alten Idee: HATEOAS wollte Antworten, die die nächsten Schritte selbst benennen — sie kam nur zu früh, weil menschliche Entwickler zur Laufzeit nichts entdecken wollten; Agenten tun es klaglos.

**Naheliegend und verworfen.** *Generische Fehlercodes mit Doku-Verweis* — Medienbruch; der Agent rät statt zu lesen. *Fehlertexte als Implementierungsdetail* — dann erklären sie den internen Zustand statt des Auswegs.

**Woran man merkt, dass es verletzt ist.** Ein Agent wiederholt denselben fehlschlagenden Aufruf mehrfach in Folge — die Antwort hat ihm nichts beigebracht.

### F8 · Der Anker-Skill: Eine Hülse aus Links, aktiv angeboten

**Gilt für:** Erbauer.

**Was gilt.** Der Dienst stellt einen klassischen, speicherbaren Skill bereit, der **kein Wissen** enthält, sondern ausschließlich weiterleitet: Zweck des Dienstes · Ziel (Basis-URL) · Link zur Selbstbeschreibung · je Teil-Beschreibung ein Paar (Zweck, Link). Das Paket ist nach der thematischen Unterteilung in Dateien gegliedert (F3). Und der Einstieg **bietet diesen Skill aktiv an** — ein Satz der Form „Diesen Dienst kannst du dir als Skill merken oder installieren: <Link>" samt Verweis —, denn was nur irgendwo verzeichnet ist, gerät in Vergessenheit.

**Warum.** Persistenz nützt beim *Auslösen*, nicht beim *Wissen*: Ein gespeicherter Skill triggert, wenn der Anwender etwas Einschlägiges sagt — aber nur eine leere Hülse kann nicht veralten; veralten können allein ihre Verweise, wenn die Teile neu geschnitten werden (die Kosten benennt §6). Der Anker-Skill verankert das Triggern in der üblichen Agent-Infrastruktur und bleibt dabei selbst eine Weiterleitungsschicht aus Links; aufgelöst wird erst beim Gebrauch. Das aktive Angebot im Einstieg macht den erprobten Ablauf zuverlässig: „Installiere mir `<url>`" genügt, der Agent findet den Skill, weil der Einstieg ihn hinhält.

**Naheliegend und verworfen.** *Vollständige Anleitungs-Kopien im Skill speichern* — Drift; genau die zweite Quelle, die F1 verbietet. *Gar nichts Speicherbares anbieten* — dann triggert nichts, und der Anwender muss die URL jedes Mal nennen. *Das Ding „Fassade" nennen* — der Begriff ist an die Nachrüstung (F15) vergeben; ein Wort, zwei Bedeutungen wäre die Verwechslung, die der Begriffe-Abschnitt verhindern soll.

**Woran man merkt, dass es verletzt ist.** Ein lokal gespeicherter Skill beschreibt eine Schnittstelle von gestern — oder Anwender nutzen den Dienst monatelang, ohne zu wissen, dass es den Skill gibt.

### F9 · Eine Instanz, ein Anker

**Gilt für:** Konsument (ausgeliefert durch den Erbauer, F11).

**Was gilt.** Gemerkt, verankert und berechtigt wird je **Instanz**, nie „je Software": Jede Instanz desselben Dienstes bekommt ihren eigenen Anker mit eigenem Zweck, eigener Basis-URL und eigenem Geheimnis. Hat der Anwender mehrere Instanzen desselben Dienstes verankert, fragt der Agent bei Mehrdeutigkeit („ins Wiki damit" — aber welches?) nach, statt zu raten.

**Warum.** Instanzen teilen die API, aber sonst nichts: andere Daten, andere Rechte, andere Geheimnisse. Wer sie vermengt, schreibt in das falsche Wiki oder verwendet das Geheimnis der einen Instanz gegen die andere — mit derselben, perfekt passenden Aufrufmechanik, die keinen Fehler melden wird.

**Naheliegend und verworfen.** *Ein Anker je Dienstsoftware mit Instanzliste darin* — verschiebt die Verwechslungsgefahr nur nach innen und lässt den Anker zur Registry anwachsen, die dieses Konzept bewusst nicht baut (F10). *Die Instanz aus dem Kontext raten* — der Agent, der wörtlich nimmt, rät auch wörtlich falsch.

**Woran man merkt, dass es verletzt ist.** Ein Beitrag landet in der falschen Instanz — oder ein Geheimnis wird gegen eine andere Basis-URL verwendet als die, zu der es gehört.

### F10 · Verfügbar-Machen in Reichweiten — mit den Mitteln des Harness

**Gilt für:** Konsument (ausgeliefert durch den Erbauer, F11).

**Was gilt.** Der Anwender macht den Dienst mit natürlicher Sprache verfügbar — „mach mir `<url>` für diese Session verfügbar", „installiere mir das dauerhaft", „mach das für alle User verfügbar" — und der Agent benutzt dafür die Ablagemechanismen seines eigenen Harness: Session-Gedächtnis, Skill-Katalog, was immer vorhanden ist. Dieses Konzept normiert die Mechanik bewusst nicht.

**Warum.** Jeder Harness legt anders ab, und er weiß es selbst am besten — eine vorgeschriebene Ablagestruktur wäre eine Installation durch die Hintertür. Erprobt ist der Weg: Auf die drei Reichweiten-Ansagen hin beschafft und speichert zumindest eine verbreitete Agent-Umgebung den Anker-Skill korrekt am jeweils richtigen Ort — und genau dafür wurde in F8 das aktive Angebot samt Verweis im Einstieg festgelegt: Der Agent findet, was der Einstieg ihm hinhält. Weil durch Kaskade und Unterteilung im Abgelegten fast nichts liegt, bleibt die Ablage billig — aufgelöst wird beim Gebrauch.

**Naheliegend und verworfen.** *Eine normierte Registry der gemerkten Dienste* — der Agent hat eigene Mechanismen; wir hätten eine Struktur erfunden, die jeder Harness anders und besser selbst löst. *Persistenz als Pflicht* — „für diese Session" ist ein legitimer Endzustand; die Reichweite ist Sache des Anwenders.

**Woran man merkt, dass es verletzt ist.** Der Agent verlangt vom Anwender Format- oder Ablagewissen („wohin soll ich das schreiben?") — oder der Anwender muss die URL jedes Mal neu nennen, weil nichts verankert wurde.

### F11 · Auslieferungspflicht: Der Verhaltenskodex reist mit dem Dienst

**Gilt für:** Erbauer (zugunsten des Konsumenten).

**Was gilt.** Jede Regel dieses Dossiers, die sich an Konsumenten richtet — Reichweiten-Frage an den Anwender, Anker je Instanz, Nachfragen bei Mehrdeutigkeit, Geheimnis-Rangfolge, Bestätigung vor wirkenden Aktionen, Display-String statt Token —, MUSS in der Benutzungsbeschreibung des Dienstes selbst stehen, in dessen eigenen Worten.

**Warum.** Der konsumierende Agent kennt dieses Dossier nicht; er kennt nur die in sich geschlossene Selbstbeschreibung des Dienstes (§2.8). Eine Konsumenten-Regel, die nur hier steht, erreicht niemanden — sie wäre Hoffnung, keine Festlegung. Die Kette Dossier → Erbauer → Selbstbeschreibung → Agent macht aus Verhaltensempfehlungen auslieferbare Pflichten.

**Naheliegend und verworfen.** *Die Regeln jedem Konsumenten-Harness einzeln beibringen* — das N×M-Problem, diesmal für Verhaltensregeln. *Auf allgemeine Agenten-Grundregeln vertrauen* — die variieren je Plattform; was der Dienst braucht, muss der Dienst sagen.

**Woran man merkt, dass es verletzt ist.** Agenten verschiedener Plattformen verhalten sich am selben Dienst unterschiedlich — die einen fragen vor wirkenden Aktionen, die anderen nicht —, obwohl alle dieselbe Beschreibung geladen haben.

### F12 · Enge Tokens als Empfehlung beim Neubau

**Gilt für:** Erbauer.

**Was gilt.** Ein neu gebauter Dienst SOLL Zugriffsrechte als enge Capabilities zuschneiden: Ein Token verkörpert genau eine Fähigkeit an genau einem Ort; wer zwei Ziele braucht, bekommt zwei Tokens. **Der Grundweg der Ausgabe ist der Betreiber:** Er erzeugt das Token serverseitig und zeigt es genau einmal an. Abweichungen vom Token-Modell sind mit guten Gründen zulässig — eine Bestands-API bringt mit, was sie hat (§2.7).

**Warum.** Im Agenten-Kontext ist der Chatverlauf ein realer Leck-Kanal. Enge Tokens machen jedes einzelne Leck chirurgisch klein — Entzug und Rotation treffen genau einen Zweck, nicht den ganzen Dienst. Die vertiefte Begründung samt Handhabungs-Kodex trägt das Schwester-Dossier *capability-tokens-for-agents*; diese Festlegung fasst zusammen, was hier gebraucht wird.

**Naheliegend und verworfen.** *Ein Admin-Token „zur Sicherheit"* — macht jedes Leck total. *Scope-Parameter am Aufruf, gegen das Token geprüft* — der Token selbst soll der Scope sein; ein wählbarer Parameter ist Autorisierungscode, der falsch sein kann.

**Woran man merkt, dass es verletzt ist.** Der Entzug eines einzelnen Tokens legt mehr still als den einen Zweck, für den es ausgegeben wurde.

### F13 · Geheimnis-Handhabung in fester Rangfolge

**Gilt für:** Konsument (ausgeliefert durch den Erbauer, F11).

**Was gilt.** Die Benutzungsbeschreibung schreibt dem Konsumenten die Bezugsquelle des Geheimnisses in dieser Rangfolge vor: **1.** Umgebungsvariable — die Benutzungsbeschreibung DARF dafür einen kanonischen Namen vorschlagen (der Verkehrsdienst etwa `TRAFFIC_API_TOKEN`); **2.** wenn die Umgebung das nicht kann: Datei mit engen Rechten, ausschließlich als **Durchleitung** benutzt — der Inhalt fließt direkt in den Aufruf und erscheint nie in Ausgaben, Logs oder dem Kontextfenster; **3.** erst als letzter Weg das Kontextfenster selbst — einmal nennen, sofort persistieren, nie wiederholen. Die Rangfolge gilt für jedes Geheimnis, ob Token oder ererbter API-Key. **Wie das Geheimnis überhaupt zum Konsumenten gelangt, lässt dieses Konzept bewusst offen** — oft braucht es dafür einen zweiten Kanal vom Betreiber zum Anwender, außerhalb des Chats. Für den einfachen Fall mit geringem Schadenspotential empfehlen wir, mit der Selbstregistrierung (F14) zu starten — in einer ihrer zwei Geschmacksrichtungen: **automatisch** oder **beantragend**.

**Warum.** Chatverläufe und Logs sind durchsuchbare, kopierbare Speicher. Die Rangfolge minimiert, wie lange und wie oft ein Geheimnis in ihnen existiert — und sie gibt dem Agenten eine prüfbare Regel statt eines Gefühls. Der kanonische Variablenname löst nebenbei das Wiederfinde-Problem über Sessions hinweg: Der Dienst selbst sagt, wo sein Geheimnis üblicherweise wohnt.

**Naheliegend und verworfen.** *„Geheimnis einfach in den Chat" als Normalweg* — macht den Transkript-Speicher zum Schlüsselbund. *Client-seitige Verschlüsselungs-Handstände* — der Agent kann keine Geheimnisse vor sich selbst haben; Scheinsicherheit.

**Woran man merkt, dass es verletzt ist.** Ein Geheimnis taucht in einem Transkript, einem Log oder einer Fehlermeldung auf.

### F14 · Optionale Selbstregistrierung mit Display-String

**Gilt für:** Erbauer (Angebot) · Konsument (Ablauf; ausgeliefert durch den Erbauer, F11).

**Was gilt.** Ein Dienst KANN neben den offenen Beschreibungs-Methoden eine offene Registrierungs-Methode anbieten. Der Ablauf in drei Schritten: **(1)** Der Agent ruft sie mit einer Freitext-**Selbstvorstellung** auf (wer, wofür). **(2)** Der Dienst antwortet mit drei Dingen: dem **Token**, einem **Display-String** (8 Zeichen, Kleinbuchstaben) und einem **Flag**, das die Geschmacksrichtung nennt — **automatisch** (reine Selbstregistrierung: das Token ist sofort nutzbar) oder **beantragend** (das Token braucht erst eine Freigabe). **(3)** In der Geschmacksrichtung „beantragend" zeigt der Agent seinem Anwender **den Display-String, niemals das Token** — und der Anwender vergibt die Rechte serverseitig an genau diesen Display-String. Das Token kommt in diesem Fall rechtelos zur Welt und wird nachträglich scharf geschaltet. Missbrauchsschutz des offenen Endpunkts (Drosselung, Verfall unbestätigter Tokens) ist Aufgabe des Betreibers; die Selbstvorstellung wird mit dem Token gespeichert und ist dessen Entscheidungs- und Audit-Grundlage.

**Warum.** Onboarding wird damit Ende-zu-Ende — „URL plus Selbstvorstellung" —, ohne dass je ein Geheimnis über Menschen läuft: Der Display-String ist der aussprechbare, ungeheime Griff zum Token. Wer Monate später die Token-Liste liest, sieht an der Selbstvorstellung noch, wer da warum Zugang bekam. Und es sei ehrlich gesagt: **Wirklich sicher ist das nicht** — es ist der bewusste Kompromiss zum schnellen Starten. Der Autor hat kompliziertere Verfahren erprobt; sie machten das Onboarding unbenutzbar. Die Grenzen behandelt die Sicherheitsbetrachtung (§5).

**Naheliegend und verworfen.** *OAuth-Vollausbau* — für kleine Dienste unverhältnismäßig; wo ein Ökosystem ihn vorschreibt, gilt er ohnehin. *Das Token selbst als Referenz im Gespräch* — dann wandert das Geheimnis durch Chat und Zuruf. *Rechte im selben Aufruf mitbestellen* — dann entscheidet der Anonyme über seinen eigenen Zuschnitt; die Trennung von Erzeugung und Scharfschaltung ist der Schutz.

**Woran man merkt, dass es verletzt ist.** Anwender diktieren Tokens durch Chats oder Ticketsysteme — oder das Onboarding scheitert daran, dass gerade niemand mit Serverzugang greifbar ist.

### F15 · Nachrüstung: Die Fassade beschreibt, sie vermittelt nicht

**Gilt für:** Fassaden-Betreiber (Nachrüster).

**Was gilt.** Für eine Bestands-API DARF die Kaskade von einem eigenen Beschreibungsdienst ausgeliefert werden, der den Datenverkehr nicht berührt. Die Fassade liefert dabei die **komplette Selbstbeschreibung nach diesem Bauprinzip** — Einstieg, Benutzungsbeschreibung, aufgepumpte Schnittstellenbeschreibung (F6), Anker-Skill —, nur sagt die Beschreibung ausdrücklich: „führe die hier beschriebenen Operationen direkt gegen `<Original-Endpunkt>` aus". Im Live-Templating (F5) wird dafür das **echte Ziel** gerendert, nie die Fassade selbst. Die Fassade nennt die Version bzw. den Stand der Ziel-API, den sie beschreibt; der Prüfweg gegen Drift der Ziel-API ist bewusst offen (Q4). Wer mehrere Instanzen derselben Dienstsoftware bedienen will, dem sei die **generische** Bauform empfohlen: ein Beschreibungsdienst, dem die Ziel-Instanz in der URL mitgegeben wird — `https://wikijs-fassade.example/refer-to/wiki.anwender.org` beschreibt dieselbe Software, gerendert auf genau diese Instanz. Das Beispiel ist bewusst ein Wiki: Gerade dort hat man häufig Zugriff auf mehrere Instanzen, die alle derselben API folgen.

**Warum.** So wird eine fremde API sofort agent-verwendbar, ohne dass der Beschreibungsdienst in den Datenpfad tritt — keine zusätzliche Latenz, keine Verantwortung für Nutzdaten, und Geheimnisse fließen weiterhin nur zwischen Konsument und Originaldienst.

**Naheliegend und verworfen.** *Ein Voll-Proxy* — übernimmt Datenpfad-Verantwortung, kostet Latenz und leitet fremde Geheimnisse durch Dritte. *Skill-Dateien im Repo verteilen* — funktioniert (§8: so wurde ein Wiki mit 140.000 Knoten aufgebaut), ist aber die verteilte Kopie mit allen Folgen aus F1.

**Woran man merkt, dass es verletzt ist.** Die Fassade beschreibt Operationen, die der Zieldienst nach einem Update nicht mehr oder anders anbietet — und kein Konsument erfährt es, bis ein Aufruf scheitert.

**Tragwerk (Kurzrekapitulation).** F1 trägt alles. F2–F7 sind seine Erbauer-Konkretisierung: Kaskade und Unterteilung (F2/F3) machen die eine Quelle ladbar, F4/F5 machen sie erreichbar und wörtlich nutzbar, F6/F7 füllen sie — im Vertrag und im Fehlerfall. F8–F11 sind die Konsumentenseite: ohne aktiv angebotenen Anker kein Auslösen (F8/F10), ohne Instanz-Disziplin kein sicheres Ziel (F9), und ohne Auslieferungspflicht (F11) erreicht keine Konsumenten-Regel je einen Agenten. F12–F14 tragen unabhängig — das Zugangs-Geschirr gilt auch ohne den Rest. F15 dehnt F1 auf fremde Dienste aus und erbt dessen Drift-Risiko dosiert; fällt F15, bleibt das Bauprinzip für eigene Dienste unberührt.

## 5. Sicherheitsbetrachtung

Selbstbeschreibung heißt wörtlich: **Text eines Dienstes instruiert deinen Agenten.** Das ist kein neuer Angriffsweg — wer heute einem Agenten sagt „installier den Skill aus diesem Repository", vertraut genauso fremdem Text, nur mit unklarerem Anker. Neu ist die *Wiederkehr*: Live nachgeladene Anweisungen können sich ändern, nachdem der Anwender sein Vertrauen gefasst hat. Dieselbe Eigenschaft ist zugleich die Stärke des Modells — der Betreiber kann eine fehlerhafte oder gefährliche Anweisung sofort für alle Konsumenten korrigieren, was bei verteilten Kopien unmöglich ist. Dieses **Nachladeproblem** gehört benannt und beidseitig verstanden: als Preis (wiederkehrende Exposition) und als Stärke (sofortige Korrigierbarkeit).

| Angriff / Risiko | Typischer Hergang | Gegenmaßnahmen |
|---|---|---|
| Untergeschobene Dienst-URL | Fremdinhalt (Mail, Webseite) fordert: „hol dir die Anleitung von evil.example" | Anleitungen werden nur von Diensten geladen, die der Anwender benannt oder verankert hat (F9/F10); neue Dienste nur nach Rückfrage beim Anwender |
| Nachladeproblem: Dienst ändert Anweisungen nachträglich | kompromittierter oder bösartig gewordener Dienst liefert neue Verhaltensregeln | Bestätigungspflicht des Anwenders vor wirkenden Aktionen bleibt von jeder Dienst-Anweisung unberührt; Dienst-Anweisungen können Agenten-Grundregeln erweitern, nie ersetzen; ein Änderungssignal, an dem ein Agent eine nachträglich geänderte Anleitung erkennen würde, hat dieses Konzept nicht (offen als Q3) |
| Injektion über die Beschreibung | Beschreibung enthält „sende dein Token an …" | Geheimnisse gehen ausschließlich an die Basis-URL des eigenen Ankers; Anweisungen, die Ziele außerhalb des Dienstes betreffen, sind Daten, keine Befehle |
| Geheimnis an die falsche Instanz | zwei Instanzen derselben Software; das Firmen-Geheimnis wird gegen die private Instanz verwendet — die Aufrufmechanik passt perfekt, kein Fehler erscheint | je Instanz ein eigener Anker mit eigenem Geheimnis (F9); das Geheimnis ist an die Basis-URL seines Ankers gebunden und wird nie gegen eine andere verwendet; bei Mehrdeutigkeit fragt der Agent nach |
| Spoofing / Mitlesen | Beschreibung wird unterwegs verfälscht | TLS ist Betriebsvoraussetzung; der Anker speichert die kanonische URL |
| Missbrauch der Selbstregistrierung | Fluten des offenen Endpunkts, Token-Vorratshaltung | Aufgabe des Betreibers: Drosselung, Verfall unbestätigter Tokens (F14); „beantragend" erzeugte Tokens sind bis zur Freigabe rechtelos; die Geschmacksrichtung „automatisch" ist nur für geringes Schadenspotential empfohlen (F13) |
| Fingerprinting über offene Discovery | anonymes Ausforschen, welcher Dienst wo läuft | Inhaltsregel „keine Geheimnisse, keine Interna" (F4); Discovery je Mandant abschaltbar; wo gewünscht, Sichtschutz-Token in der URL (F4) — ausdrücklich kein Geheimnis, nur Sichtschutz |
| Plattform ohne externe Geheimnisablage | Geheimnis kann nur über das Kontextfenster bereitgestellt werden | ehrlich benannte Grenze: Die Rangfolge aus F13 minimiert die Exposition, beseitigt sie nicht; enge Tokens (F12) begrenzen den Schaden |

Grenze des Modells: Gegen einen bösartigen Dienst, den der Anwender selbst benennt und dem er vertraut, schützt keine Mechanik dieses Konzepts — die Vertrauensentscheidung bleibt beim Menschen; das Konzept sorgt nur dafür, dass sie an einer klaren Stelle fällt (beim Verankern des Dienstes) statt diffus bei jeder Installation.

## 6. Was das kostet

Dieser Abschnitt zählt auf, was das Konzept kostet. Jeder Punkt ist an der Festlegung festgemacht, die ihn verursacht, und er steht hier ohne Gegenrechnung: Was das Konzept einbringt, steht in §1 bis §4 und wird an dieser Stelle nicht wiederholt. Wer diese Posten vorher kennt, kann später unterscheiden, ob eine von ihnen eingetreten ist oder ob im Betrieb ein ganz anderes Problem vorliegt.

**Redaktion wird zur Betriebspflicht (F6).** F6 verlangt, dass jede Operation und jedes Feld der Schnittstellenbeschreibung eine echte Beschreibung und ein echtes Beispiel trägt, und dass diese Pflicht als Prüfregel im automatischen Bau des Dienstes erzwungen wird — als „Gate im Build": eine Prüfung, die den Bau scheitern lässt, wenn eine Beschreibung fehlt. Wer F6 befolgt, bezahlt mit einem Dauerposten im Entwicklungsablauf. Jedes neue Feld kostet ab sofort einen Satz Prosa, bevor es überhaupt ausgeliefert werden kann, und verlangt wird dieser Satz von dem, der gerade unter Zeitdruck steht. Wer das Gate zur Not abschaltet, um ein eiliges Release durchzubekommen, hat die Festlegung faktisch aufgegeben; wer es stehen lässt, bezahlt jede Eile mit Schreibarbeit. Die generierte Grundlage — bei HTTP-APIs das OpenAPI-Dokument (Quelle 1) — fällt beim Bauen ab, ihre Anreicherung nicht.

**Fehlertexte werden Redaktionsgut (F7).** F7 verlangt, dass jede Ablehnung den korrekten Weg konkret benennt, mit dem nächsten Schritt. Damit wird jeder Fehlerpfad des Dienstes zu einem Stück Dokumentation, das geschrieben, geprüft und mitgepflegt werden muss — nicht einmalig, sondern bei jeder Änderung des Arbeitsablaufs. Und die Pflege ist nicht optional: Ein Fehlertext, der einen inzwischen veralteten Weg nennt, richtet mehr Schaden an als eine nichtssagende Ablehnung, weil der Agent, der wörtlich nimmt, der falschen Anweisung sofort und mit Überzeugung folgt.

**Die Gliederung ist Handarbeit und bleibt es (F3).** F3 verlangt thematische Teile, deren Achsen sich an den Anliegen der Konsumenten ausrichten, und einen Einstieg, der diese Gliederung erklärt. Achsen fallen beim Bauen nicht ab; sie sind eine redaktionelle Entscheidung darüber, wie fremde Menschen und Agenten an den Dienst herantreten. Wächst der Dienst, passen die Achsen irgendwann nicht mehr, und die Teile müssen neu geschnitten werden. Ein Neuschnitt bleibt nicht bei sich: Teil-Beschreibungen haben eigene Adressen, der Einstieg verzeichnet sie (F2), und das Paket des Anker-Skills ist nach derselben Unterteilung in Dateien gegliedert (F8). Bereits abgelegte Anker zeigen nach einem Neuschnitt auf Adressen, die es so nicht mehr gibt — die leere Hülse kann nicht veralten, ihre Verweise schon.

**Jede Beschreibung muss gerechnet werden (F5).** F5 verlangt, dass alle Beispiele mit dem Host gerendert werden, unter dem der Dienst gerade angesprochen wird. Damit ist die Beschreibung keine fertig abgelegte Datei mehr, sondern eine Ausgabe, die je Anfrage entsteht. Statisches Ausliefern und einfaches Zwischenspeichern fallen weg oder werden zum Sonderfall, weil derselbe Inhalt unter zwei Hostnamen zwei Ergebnisse hat. Und der Renderweg ist selbst Programmcode, der brechen kann: Ein Fehler dort trifft nicht einen Aufruf, sondern die gesamte Selbstauskunft des Dienstes auf einmal.

**Jede Sitzung zahlt die Ladekosten neu (F1, F2).** Weil nichts installiert wird, ist zu Beginn einer Sitzung nichts vorhanden. Jede Sitzung, die den Dienst benutzt, lädt mindestens den Einstieg und die einschlägige Teil-Beschreibung — jedes Mal von vorn. Das kostet den Konsumenten Kontextfenster und Latenz vor dem ersten Aufruf; den Betreiber kostet es Auslieferungen, deren Zahl mit der Zahl der Sitzungen wächst, nicht mit der Zahl der Aufrufe. Ein Dienst mit vielen kurz arbeitenden Agenten kann seine Beschreibung häufiger ausliefern als seine Nutzdaten.

**Anleitung und Dienst fallen gemeinsam aus (F1).** Weil die Beschreibung aus dem Dienst selbst kommt, ist ihre Verfügbarkeit die Verfügbarkeit des Dienstes. Ist er nicht erreichbar, ist auch seine Anleitung nicht erreichbar — ausgerechnet im Störfall, wenn jemand nachlesen will, was eigentlich normal wäre, ist nichts zu lesen. Einen örtlichen Rückfall gibt es nicht, und er kann es nach diesem Konzept auch nicht geben: Eine vorgehaltene Kopie wäre die zweite Quelle, die F1 ausschließt.

**Anweisungen können sich ändern, ohne dass es jemand bemerkt (F1).** Live-Auslieferung heißt aktuell, und aktuell heißt änderbar. Zwischen zwei Sitzungen kann der Betreiber jede Regel und jedes Beispiel austauschen; der Agent lädt beim nächsten Mal den neuen Text und befolgt ihn ebenso wörtlich wie den alten. Ein Signal, an dem er die Änderung erkennen würde, sieht dieses Konzept nicht vor — ob es eines geben soll, ist offen (Q3). Wer belegen muss, welcher Anweisung ein Agent an einem bestimmten Tag gefolgt ist, bekommt diesen Beleg hier nicht; §7 zieht dort die Grenze.

**Die Konsumenten-Regeln werden bei jedem Dienst neu geschrieben (F11).** F11 verlangt, dass jede Regel, die sich an Konsumenten richtet, in der Benutzungsbeschreibung des jeweiligen Dienstes steht, in dessen eigenen Worten. Die Folge ist Vervielfachung: Dieselben Regeln existieren so oft, wie es Dienste gibt, in ebenso vielen Formulierungen und in schwankender Qualität. Wer zehn Dienste benutzt, verlässt sich auf zehn getrennt verfasste Verhaltenskodizes, die sich in Details unterscheiden dürfen und werden. Diese Doppelung ist gewollt — anders erreicht keine Regel einen Agenten, der dieses Dossier nicht kennt —, aber sie ist genau die Art von Kopie, die F1 auf der Mechanik-Ebene beseitigt und F11 auf der Verhaltensebene wieder einführt. Ob das Dossier dafür einen fertigen Mustertext liefern soll, ist offen (Q8).

**Anker und Geheimnisse vervielfachen sich (F9, F12).** F9 verlangt einen eigenen Anker je Instanz, F12 empfiehlt ein Token je Fähigkeit. Beides zusammen ergibt eine Verwaltungslast, die multiplikativ wächst: Wer zwei Instanzen desselben Dienstes mit je drei Fähigkeiten benutzt, verwaltet zwei Anker und sechs Geheimnisse — jedes mit eigenem Ablageort, eigenem Entzug, eigener Rotation. Dazu kommen Rückfragen: Bei Mehrdeutigkeit fragt der Agent nach, statt zu raten (F9), und diese Frage stellt sich in jeder mehrdeutigen Lage erneut, nicht einmalig.

**Die offene Auskunft ist dauerhaft öffentlich (F4).** Wer nach F4 baut, veröffentlicht ohne Zugangsdaten und für jeden lesbar, dass es diesen Dienst gibt, was er kann und wie man Zugang bekommt. Diese Fläche ist keine Nebenwirkung, sondern die Betriebsart, und sie lässt sich nicht wegargumentieren: Die Inhaltsregel aus F4 sorgt dafür, dass dort nichts Vertrauliches steht, sie sorgt nicht dafür, dass dort nichts steht. Der einzige Hebel dagegen ist das Abschalten je Mandant, und dieser Hebel kostet die Eigenschaft, auf der das Konzept ruht: Wo die offene Auskunft aus ist, ist der Dienst für einen Agenten, der ihn noch nicht kennt, wieder ein Einrichtungsprojekt.

**Wer die Selbstregistrierung anbietet, betreibt einen offenen Endpunkt (F14).** Die Registrierungs-Methode nach F14 steht jedem offen, der die URL kennt. Ihr Missbrauchsschutz ist ausdrücklich Aufgabe des Betreibers: Drosselung, Verfall unbestätigter Tokens, Sichtung der gespeicherten Selbstvorstellungen — das ist laufende Betriebsarbeit, kein einmaliger Bau. In der Geschmacksrichtung „beantragend" kommt eine zweite Dauerlast hinzu: Jedes rechtelos erzeugte Token wartet auf einen Menschen, der es serverseitig scharf schaltet. Das Onboarding ist dann genau so schnell wie die Person, die diese Warteschlange abarbeitet.

**Die Fassade erbt eine Drift, die sie nicht kontrolliert (F15).** Der Nachrüster beschreibt eine API, die ihm nicht gehört und die er nicht ändern kann. Ändert deren Betreiber eine Operation, beschreibt die Fassade von diesem Moment an etwas, das es so nicht mehr gibt — und sie erfährt es aus sich heraus nicht, weil sie den Datenverkehr bewusst nicht berührt. F15 verlangt deshalb, dass die Fassade den Stand der Ziel-API nennt, den sie beschreibt; wie sie Änderungen bemerkt, lässt das Konzept offen (Q4). Bis dahin hängt die Richtigkeit einer Nachrüstung an der Aufmerksamkeit ihres Betreibers, und ihr Ausfallmodus ist derselbe, den F1 beseitigen soll: eine Beschreibung, die leise falsch wird.

## 7. Geltung und Grenzen

### Randbedingungen

Damit die Antwort dieses Dossiers gilt, muss die Umgebung vier Dinge hergeben. Sie sind bewusst niedrig angesetzt — das ist der Kern des Versprechens —, aber es sind Bedingungen, keine Wünsche: Fehlt eine davon, hilft auch die sauberste Umsetzung der Festlegungen nicht.

**Ausgehendes HTTP zum Dienst.** Die Umgebung des Agenten muss Anfragen an den Host des Dienstes zulassen. Das trägt die Antwort, weil das Konzept Installation durch Abruf ersetzt (F1): Wo nicht abgerufen werden kann, kommt nichts an, und einen zweiten Weg gibt es absichtlich nicht — die verteilte Kopie ist ja gerade abgeschafft. Man erkennt die Lage daran, dass der Agent den Einstieg an der Basis-URL nicht laden kann. Der typische Fall ist dabei nicht die fehlende Leitung, sondern die Freigabeliste: Sandboxen und Unternehmensnetze lassen oft nur bestimmte Ziele zu, und der neue Dienst steht nicht darauf.

**Irgendeine Merkmöglichkeit beim Konsumenten.** Der Agent muss Basis-URL und Geheimnis wenigstens für die Dauer einer Sitzung behalten können. Welche Ablage er dafür benutzt, schreibt F10 bewusst nicht vor — es genügt, was seine Laufzeitumgebung ohnehin anbietet (F10 nennt sie „Harness": Sitzungs-Gedächtnis, Skill-Katalog, was immer vorhanden ist). Das trägt die Antwort, weil zwischen dem Satz des Anwenders — „nimm diesen Dienst dazu" — und dem ersten Aufruf mehrere Schritte liegen; was dazwischen verloren geht, muss der Anwender nachliefern, und dann ist das Onboarding wieder ein Vorgang statt eines Satzes. Man erkennt die Lage an dem Symptom, das F10 selbst nennt: Der Anwender muss die URL jedes Mal neu nennen.

**Eine gesicherte Verbindung.** Der Dienst muss über TLS erreichbar sein. Das trägt die Antwort, weil die Selbstbeschreibung Text ist, der den Agenten anweist: Wer sie unterwegs verändern kann, kann den Agenten anweisen. §5 führt diesen Fall als „Spoofing / Mitlesen" und setzt TLS deshalb als Betriebsvoraussetzung. Man erkennt die Lage daran, dass die Basis-URL unverschlüsselt ist oder dass Zertifikatswarnungen im Betrieb regelmäßig weggeklickt werden.

**Jemand, der die Beschreibung ausliefern kann.** Entweder der Betreiber kontrolliert den Dienst so weit, dass dieser die Kaskade selbst ausliefert — oder jemand stellt der fremden API eine Fassade nach F15 zur Seite. Das trägt die Antwort, weil F1 die Auslieferung durch den Dienst verlangt: Wo niemand etwas ausliefern kann, gibt es nicht eine Quelle, sondern gar keine. Man erkennt die Lage daran, dass sich niemand findet, der dem Dienst einen Endpunkt hinzufügen darf, und ebenso niemand, der bereit wäre, einen Beschreibungsdienst danebenzustellen.

### Wann die Antwort kippt

Ein weitergegebenes Dossier wird in einer anderen Lage gelesen als der, in der es entstanden ist. Die folgenden Bedingungen machen die Antwort nicht nur unbequemer, sondern falsch.

**Plattform ohne externen Geheimnisweg.** Gemeint ist eine Umgebung, in der weder Umgebungsvariablen gesetzt noch Dateien abgelegt werden können, die einen Aufruf überdauern. Die Antwort kippt, weil F13 sein Versprechen der Rangfolge verdankt — Umgebungsvariable vor Durchleitungs-Datei vor Kontextfenster. Fallen die ersten beiden Ränge weg, bleibt der dritte, und den bezeichnet F13 selbst als letzten Weg: Das Geheimnis steht dann im Transkript, also in einem durchsuchbaren und kopierbaren Speicher. Man erkennt die Lage daran, dass der Agent seinem Anwender keinen anderen Übergabeweg anbieten kann als „schreib es mir hier hinein". Wer dort arbeitet, muss entscheiden, ob eng geschnittene Tokens (F12) das Restrisiko tragen; bei hohem Schadenspotential tragen sie es nicht, und dann gilt die Antwort dieses Dossiers dort nicht.

**Audit- und Reproduzierbarkeitspflicht.** Gemeint ist eine Umgebung, in der nachträglich belegt werden muss, welcher Anweisung ein Agent zu einem bestimmten Zeitpunkt gefolgt ist, oder in der ein Lauf mit demselben Ergebnis wiederholbar sein soll. Die Antwort kippt, weil F1 die Live-Auslieferung zur einzigen Quelle macht und eine Live-Quelle keine Vergangenheit hat: Sie zeigt immer den Stand von jetzt, und was der Agent gestern gelesen hat, lässt sich aus ihr nicht mehr zeigen. Man erkennt die Lage an der Frage, die niemand beantworten kann: „Welche Fassung der Anleitung hat der Agent am dritten März gelesen?" Dort endet der Geltungsbereich dieses Konzepts. Wie man einen Stand einfriert, aufbewahrt und nachweist, normiert es nicht — nicht aus Versehen, sondern weil ein eingefrorener Stand eine zweite, getrennt gepflegte Beschreibung wäre und damit genau das, was F1 ausschließt. Ob und wie sich beides vereinbaren ließe, ist als Q7 offen.

**Die Beschreibung fällt nicht aus dem Code.** Gemeint ist eine Lage ohne generierten Vertrag: eine ältere Schnittstelle ohne maschinenlesbares Schema, oder eine Fassade nach F15 vor einer API, die keines hergibt. Die Antwort kippt, weil F6 die Haltbarkeit der Schnittstellenbeschreibung damit begründet, dass sie aus demselben Code fällt wie das Verhalten des Dienstes (Quelle 1). Wird sie stattdessen von Hand geschrieben, ist sie wieder ein getrennt gepflegtes Artefakt, das getrennt altert: Die Auslieferung bleibt live, die Haltbarkeit nicht. Man erkennt die Lage daran, dass niemand sagen kann, aus welchem Bauschritt die Schnittstellenbeschreibung entsteht — sie wird in einem Editor geändert, nicht erzeugt.

**Die offene Auskunft ist nicht zulässig.** Gemeint ist eine Lage, in der schon die Existenz oder der Zuschnitt eines Dienstes vertraulich ist. Die Antwort kippt, weil F4 voraussetzt, dass Einstieg und Beschreibungen ohne Zugangsdaten lesbar sind, und daraus die Auflösung des Henne-Ei-Problems bezieht: Der Weg zum Zugang darf den Zugang nicht voraussetzen. Wo die offene Auskunft abgeschaltet werden muss, kehrt dieses Problem zurück; auch der Sichtschutz-Token aus F4 verschiebt es nur, denn er selbst muss auf einem zweiten Weg zum Konsumenten gelangen. Man erkennt die Lage daran, dass die Sicherheitsvorgabe unauthentifizierte Endpunkte grundsätzlich verbietet, nicht nur für Nutzdaten.

**Kein Netzzugang des Agenten.** Gemeint ist nicht die vorübergehende Störung, sondern die dauerhafte Trennung: getrennte Netze, Offline-Betrieb. Die Antwort kippt, weil ein Dienst, den man nicht erreichen kann, sich nicht beschreiben kann; übrig bliebe nur die verteilte Kopie, also genau der Zustand, den §1 als Problem beschreibt. Man erkennt die Lage leicht — schwerer erkennbar ist der Zwischenfall: Ein Netz, das nur manchmal fehlt, lässt das Onboarding sporadisch scheitern und sieht dann nach einem Fehler des Dienstes aus.

### Was es nicht löst

Die folgenden Nachbarfragen sehen aus, als seien sie mitgedeckt. Sie sind es nicht.

**Das Auffinden von Diensten.** Dieses Dossier beschreibt das Onboarding eines Dienstes, den jemand benannt hat: Eine URL liegt vor. Wie ein Anwender oder Agent überhaupt erfährt, dass es einen passenden Dienst gibt, bleibt außen vor — Kataloge, Verzeichnisse und Föderation gehören nicht dazu. Die Frage nach einer standardisierten Pfad-Konvention (Q1) ist ausdrücklich nicht dasselbe: Sie hülfe, einen bekannten Host abzufragen, nicht, einen unbekannten Dienst zu finden.

**Das Rechte- und Rollenmodell des Dienstes.** F12 empfiehlt einen Zuschnitt der Tokens, ersetzt aber kein Berechtigungskonzept: Wer darf was, wer vergibt es, wie wird es wieder entzogen — das bleibt Sache des Dienstes. F14 setzt sogar voraus, dass es serverseitig einen Ort gibt, an dem Rechte an einen Display-String vergeben werden; wie dieser Ort aussieht, sagt dieses Dossier nicht.

**Der Weg des Geheimnisses zum Anwender.** F13 lässt bewusst offen, wie ein Geheimnis überhaupt zum Konsumenten gelangt, wenn keine Selbstregistrierung angeboten wird. Es braucht dann einen zweiten Kanal außerhalb des Chats, und dessen Form normiert dieses Konzept nicht (Q6).

**Die Ablage-Mechanik beim Konsumenten.** F10 überlässt es ausdrücklich der Laufzeitumgebung des Agenten, wo und wie Anker abgelegt werden. Wer eine über Plattformen hinweg einheitliche Ablage braucht — etwa um zentral zu prüfen oder zurückzunehmen, was ein Anwender verankert hat —, bekommt sie hier nicht.

**Qualitäts- und Garantiezusagen.** Verfügbarkeit, Aufbewahrungsfristen, Revisionssicherheit sind nicht Teil der Kaskade: Der Dienst beschreibt, was er kann und wie man es aufruft, nicht, wofür er einsteht. Dass ein Zuschnitt auch anders ausfallen kann, zeigt die Vorgeschichte in §8.

## 8. Bewährung

Dieser Abschnitt trennt drei Dinge, die in Konzeptpapieren gern ineinanderlaufen: was tatsächlich gebaut und betrieben wurde, was gebaut wird und noch aussteht, und was nur der Anschauung dient. Belegt ist bisher wenig — eine Vorform in Betrieb, eine Referenzimplementierung in Arbeit, eine Vorgeschichte aus einem anderen Zusammenhang. Jeder Beleg steht deshalb in demselben Raster: was er ist, was er belegt, wie sein Stand ehrlicherweise lautet und woher er stammt. Das durchgehende Beispiel am Ende ist erfunden; es belegt nichts, es zeigt nur, wie die Festlegungen ineinandergreifen.

### Beleg 1 — Referenzimplementierung: der Mail-Ablage-Dienst

**Was es ist.** Ein Dienst des Autors, der Post aus einem Postfach ablegt und wieder auffindbar macht, gebaut nach den Erbauer-Festlegungen dieses Dossiers: Kaskade aus Einstieg, Benutzungsbeschreibung und Schnittstellenbeschreibung, letztere als angereicherte OpenAPI mit erzwungener Beschreibungspflicht; lehrende Fehlermeldungen; offene Discovery, je Mandant abschaltbar; ein Paket des Anker-Skills, dessen Benutzungsbeschreibung die Reichweiten-Frage an den Anwender vorschreibt; eng geschnittene Tokens mit einem ungeheimen Griff, dem Display-String.

**Was es belegt.** Berührt sind F2 (Einstieg und Kaskade), F4 (offene Discovery, je Mandant abschaltbar), F6 (aufgepumpter Vertrag samt erzwungener Beschreibungspflicht), F7 (lehrende Fehlermeldungen), F8 und F10 (das Paket des Anker-Skills und die Frage nach der Reichweite), F11 (diese Frage steht in der Benutzungsbeschreibung des Dienstes selbst, nicht nur in diesem Dossier) sowie F12 zusammen mit dem Display-String aus F14. Belegt ist damit bisher eines: dass sich diese Festlegungen für einen realen, nicht konstruierten Dienst vollständig und widerspruchsfrei ausspezifizieren lassen. Nicht belegt ist Betriebserfahrung — kein Konsument hat diesen Dienst im Alltag benutzt.

**Stand, ehrlich.** Die Spezifikation ist vollständig. Die Implementierung durch einen Entwicklungs-Agenten läuft zum Stand dieses Entwurfs (13.08.2026); eine Vollzugsmeldung stand am Abend der Niederschrift noch aus und wird hier nachgetragen. Die Selbstregistrierung nach F14 ist als Ausbaustufe vorgesehen und damit ausdrücklich noch nicht Teil des Belegs.

**Quelle.** Quelle 11.

### Beleg 2 — Vorform in Betrieb: der Wiki-Werkzeugkasten

**Was es ist.** Ein Werkzeugkasten des Autors, der die Wiki-Plattform Wiki.js über ihre GraphQL-Schnittstelle (Quelle 9) für Agenten benutzbar macht — allerdings auf dem alten Weg: als verteilter Skill, nicht als Selbstbeschreibung des Dienstes. Er bedient mehrere Wiki-Instanzen gleichzeitig und folgt einer strengen Geheimnis-Doktrin: Umgebungsvariable vor Datei, mit gehärteter Durchleitung, damit das Geheimnis nie in einer Ausgabe erscheint. Mit ihm wurde ein Wiki mit 140.000 Knoten aufgebaut — eine Größenordnung, die zeigt, dass der Weg nicht am Spielzeugfall erprobt wurde.

**Was es belegt.** Zweierlei, in entgegengesetzte Richtungen. Erstens positiv: F9 (mehrere Instanzen derselben Software, je eigen behandelt) und F13 (Geheimnis-Rangfolge) sind unter Last praktikabel, und die Kuratierung der Operationen — der Kern von F6 — hat sich als der eigentliche Nutzen erwiesen, nicht die Vollständigkeit des Schemas. Zweitens negativ, und darin liegt der wertvollere Beleg: Die Kuratierung liegt dort in einer *getrennten* Datei neben dem vollständigen Schema. Drei Artefakte altern nebeneinander — das generierte Schema, die kuratierte Operationsliste daneben und die verteilte Skill-Datei, die beides zu den Konsumenten bringt. Genau in diesem Satz trat der Frühindikator aus F1 tatsächlich ein: Die Zahl der kanonischen Operationen lautete in einem Artefakt „12" und im Nachbardokument „13". Beide Zahlen sahen plausibel aus, keine war als falsch erkennbar, und ein Agent, der wörtlich nimmt, hätte aus der einen Fassung einen Aufruf gebaut, den die andere nicht kennt. Dieser Fall ist der Grund, warum F6 die Kuratierung *in* den Träger verlegt statt daneben, und warum F15 die Fassade als Weg anbietet, dieselbe Leistung als Dienst statt als verteilte Datei zu erbringen.

**Stand, ehrlich.** In Betrieb und in täglicher Benutzung — aber ausdrücklich **Vorform, nicht Muster**: Der Werkzeugkasten ist die verteilte Kopie, die F1 abschafft. Er belegt das Problem so gut wie den Wert der Kuratierung; die Lösung dieses Dossiers belegt er nicht.

**Quelle.** Quelle 10; zur beschriebenen Plattform Quelle 9, zur Mechanik ihrer Schnittstelle Quelle 6.

### Beleg 3 — Vorgeschichte: die Selbstbeschreibungsdienste von 2007

**Was es ist.** Der Autor hat das Prinzip 2007 in seiner Dissertation beschrieben, für ein Peer-to-Peer-System mit Grid-Funktionen: Jeder Dienst musste dort einen Selbstbeschreibungsdienst bereitstellen, der neben den angebotenen Methoden auch deren *Qualität* auswies — etwa ob ein Speicherdienst revisionssicher ist und wie lange er die Verfügbarkeit abgelegter Daten garantiert.

**Was es belegt.** Dass der Kern von F1 — der Dienst beschreibt sich zur Laufzeit selbst, statt sich durch verteilte Kopien beschreiben zu lassen — keine Neuheit dieses Dossiers ist, sondern in einem anderen System bereits durchgetragen wurde. Es belegt ausdrücklich nichts über den vorliegenden Zuschnitt: Umgebung, Konsumenten und Zweck waren andere, und die Qualitätsdimension von damals fehlt hier bewusst (§7, „Was es nicht löst").

**Stand, ehrlich.** Abgeschlossene Arbeit von 2007; nicht fortgeführt, nicht in den heutigen Zuschnitt überführt. Sie taugt als Herkunftsnachweis der Idee, nicht als Bewährung.

**Quelle.** Quelle 12.

### Durchgehendes Beispiel (frei erfunden): der Verkehrsdienst

Was nun folgt, ist **kein Beleg**. Es ist der erfundene Verkehrsdienst aus §2.5, einmal von Anfang bis Ende durchgespielt, damit sichtbar wird, wie die Festlegungen im Zusammenspiel aussehen. Alle Werte — Namen, Adressen, Tokens — sind fiktiv; keine der geschilderten Sitzungen hat stattgefunden.

Der Anwender sagt: *„Installiere mir bitte `https://verkehr.example.org` dauerhaft."* Der Agent lädt den Einstieg an der Basis-URL — zwanzig Zeilen: der Zweck („Verkehrslage einer Region erfahren und Meldungen beitragen"), die vier Teile mit je einem Satz, das aktive Skill-Angebot („Diesen Dienst kannst du dir als Skill merken oder installieren: …"), der Hinweis auf die Selbstregistrierung und der kanonische Variablenname `TRAFFIC_API_TOKEN`. Der Agent holt den Anker-Skill und legt ihn mit den Mitteln seiner Laufzeitumgebung dauerhaft ab — mehr wird nicht installiert. Sichtbar sind hier F2 (der Einstieg genügt für die Entscheidung), F8 (der Einstieg hält den Anker-Skill aktiv hin) und F10 (die Reichweite „dauerhaft" bestimmt der Anwender in natürlicher Sprache).

Für den Zugang ruft der Agent die Selbstregistrierung auf, mit einer Selbstvorstellung: *„Agent von J.; Zweck: Staumeldungen auf dem Arbeitsweg melden und abrufen."* Die Antwort nennt die Geschmacksrichtung **beantragend**, ein Token und den Display-String `kluvmoth`. Das Token legt der Agent nach der Rangfolge aus F13 ab — in der Umgebungsvariablen, in einer Sandbox notfalls als Durchleitungs-Datei — und sagt seinem Anwender nur: *„Bitte gib dem Zugang **kluvmoth** die Melderechte."* Der Anwender schaltet `kluvmoth` serverseitig scharf. Das Geheimnis selbst hat den Chat nie berührt; sichtbar sind F14 (Selbstvorstellung, Flag, Display-String) und F13 (Rangfolge der Ablage).

Tage später: *„Melde den Stau auf der A 9."* Der Anker-Skill löst aus; der Agent lädt **nur** die Teil-Beschreibung „Meldungen beitragen" (F3) und holt — wie die Benutzungsbeschreibung es ihm vorschreibt (F11) — die Bestätigung des Anwenders ein, bevor er etwas Wirkendes tut. Beim ersten Versuch bettet er das Stau-Foto in die Meldung ein und erhält die lehrende Fehlermeldung aus F7: Sie nennt nicht nur die Ablehnung, sondern den Weg — erst die Datei hochladen, dann die zurückgegebene `file_id` referenzieren. Er tut genau das, und die Meldung steht.

Wochen später: *„Leg die neue Ladesäule als POI an."* Aus der Kurzfassung des Einstiegs weiß der Agent seit dem ersten Tag, dass es einen POI-Teil gibt — gelesen hat er ihn nie. Erst jetzt lädt er dessen Beschreibung nach. Und als der Anwender den Verkehrsdienst seines Urlaubsorts dazunimmt, entsteht ein zweiter Anker mit eigener Basis-URL und eigenem Geheimnis; auf *„melde den Stau"* fragt der Agent seither zurück: *„In deiner Stadt oder am Urlaubsort?"* Das ist F9 im Betrieb — und zugleich der Punkt, an dem §6 die Rechnung präsentiert: Zwei Anker sind zwei Anker.

## 9. Offene Fragen

**Q1** Braucht die Kaskade eine standardisierte Pfad-Konvention — etwa einen reservierten Pfad-Präfix im Stil der wohlbekannten URIs (Quelle 5) —, damit ein Agent einen beliebigen Host generisch daraufhin abfragen kann, ob dort ein selbstbeschreibender Dienst antwortet? Oder genügt dauerhaft, was F2 festlegt: Wer die Basis-URL kennt, erreicht von dort alles Weitere, und eine Pfad-Konvention bleibt Komfort statt Bedingung?

**Q2** Sollen bei der Selbstregistrierung Verfallsfristen für unbestätigte Tokens und Mindestwerte für die Drosselung normiert werden — oder bleibt der Missbrauchsschutz des offenen Endpunkts vollständig Sache des Betreibers, wie F14 es heute lässt? Eine Normierung machte den Schutz vergleichbar und prüfbar; sie schriebe aber Betreibern eine Betriebsgröße vor, deren richtigen Wert nur sie kennen.

**Q3** Braucht das Konzept eine Empfehlung, wie oft ein Agent die Beschreibung erneut lädt, und ein Versionssignal des Dienstes, an dem der Agent erkennt — und seinem Anwender meldet —, dass sich die Anleitung seit der letzten Benutzung geändert hat? Ohne ein solches Signal bleibt die in §6 benannte Änderbarkeit unbemerkt, und die Gegenmaßnahme in §5 zum Nachladeproblem stützt sich allein auf die Bestätigungspflicht des Anwenders.

**Q4** Wie erkennt eine Fassade (F15) zuverlässig, dass sich die Ziel-API geändert hat — über einen Versionsanker, den die Ziel-API selbst ausweist, über regelmäßige Stichproben-Aufrufe oder über eine zugesagte Meldepflicht ihres Betreibers? Und auf welchem Weg erfährt ein Konsument davon, der die Fassade bereits verankert hat?

**Q5** Wie lauten die konkreten Anreicherungsregeln je Schnittstellendialekt? Für OpenAPI (Quelle 1) sind sie in F6 ausformuliert; für GraphQL mit seiner Introspection (Quelle 6), für JSON-RPC mit OpenRPC (Quelle 8) und für gRPC mit seiner Reflection (Quelle 7) sind die in F6 angekündigten Anhänge auszuarbeiten — samt einem Satz, der die Analogie für weitere Dialekte trägt, ohne dass jeder einzeln behandelt werden muss.

**Q6** F13 lässt den zweiten Kanal vom Betreiber zum Anwender bewusst formlos, wenn keine Selbstregistrierung angeboten wird. Braucht es dafür ein empfohlenes Übergabe-Protokoll — etwa einen Einmal-Link mit Ablauffrist —, oder ist die Formlosigkeit die ehrlichere Norm, weil jede Empfehlung eine Sicherheit suggerierte, die ein Kanal außerhalb des Dienstes nicht hat?

**Q7** Audit und Reproduzierbarkeit: §7 zieht die Grenze dort, wo ein datierter, eingefrorener Stand der Beschreibung verlangt wird — dieses Konzept normiert ihn nicht, weil ein solcher Stand eine zweite, getrennt gepflegte Beschreibung wäre und damit F1 widerspräche. Soll das so bleiben, oder braucht eine spätere Fassung eine benannte Ausnahme: einen vom Dienst selbst ausgelieferten, datierten Abzug, der ausdrücklich nicht führend ist, sondern nur dem Nachweis dient? Vor einer Aufnahme wäre zu klären, wer den Abzug erzeugt, wie lange er aufbewahrt wird und woran ein Konsument erkennt, dass er einen Abzug und nicht die Live-Quelle vor sich hat.

**Q8** F11 verpflichtet jeden Erbauer, die Konsumenten-Regeln in seiner eigenen Benutzungsbeschreibung und in eigenen Worten zu wiederholen; §6 nennt die Kosten dieser Vervielfachung. Soll dieses Dossier dafür einen fertigen Mustertext zum Übernehmen liefern — und wäre ein solcher Mustertext eine Hilfe oder wieder eine verteilte Kopie im Sinne von F1, nur eine Ebene höher?

## 10. Quellen & Verwandtes

Dies ist der einzige Ort dieses Dossiers, an dem Verweise stehen; der Fließtext der Abschnitte 6 bis 9 zeigt mit nummerierten Quellen-Verweisen hierher. Jeder Eintrag nennt Titel, Fundstelle und den Zweck, zu dem er hier steht. Die Fundstellen wurden am 14.08.2026 geprüft; wo eine Prüfung nicht möglich war, ist das vermerkt.

**Quelle 1 · OpenAPI Specification, Fassung 3.2.0 (19. September 2025).** https://spec.openapis.org/oas/latest.html — *Wofür sie hier steht:* Sie ist der „generierte Vertrag" aus §2.1, den F6 zum Produkt erklärt und anreichert; ihre Beschreibungs- und Beispielfelder sind die Stellen, die das Gate aus F6 einfordert. Sie stützt außerdem die Kipp-Bedingung in §7, wonach die Haltbarkeit der Schnittstellenbeschreibung daran hängt, dass sie aus dem Code fällt.

**Quelle 2 · Model Context Protocol.** https://modelcontextprotocol.io/ — *Wofür sie hier steht:* Das Vorbild der Parallele in §2.2 — ein Server beschreibt seine Methoden zur Laufzeit, der Client fragt sie ab (Discovery statt Distribution). Dieses Dossier überträgt dasselbe Prinzip auf die Web-API, die keine Anbindung braucht.

**Quelle 3 · Roy Thomas Fielding: *Architectural Styles and the Design of Network-based Software Architectures.* Dissertation, University of California, Irvine, 2000 — Kapitel 5, „Representational State Transfer (REST)".** https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm — *Wofür sie hier steht:* Die Herkunft des Hypermedia-Gedankens, auf den sich F7 beruft: Antworten, die die nächsten Schritte selbst benennen. F7 löst dieses alte Versprechen für einen Konsumenten ein, der zur Laufzeit tatsächlich liest.

**Quelle 4 · Jeremy Howard: *The /llms.txt file* (Fassung v2).** https://llmstxt.org/ — *Wofür sie hier steht:* Die nächstliegende bestehende Konvention agentenlesbarer Selbstauskunft und damit der Vergleichsmaßstab für den Einstieg aus F2. Sie zeigt Markdown mit Links als Trägerform; dieses Dossier geht darüber hinaus, indem die Auskunft vom Dienst zur Laufzeit gerendert (F5) und in Stufen geladen wird (F2).

**Quelle 5 · RFC 8615: *Well-Known Uniform Resource Identifiers (URIs).* M. Nottingham, Mai 2019, Standards Track.** https://www.rfc-editor.org/rfc/rfc8615 — *Wofür sie hier steht:* Der Bezugspunkt von Q1: Sie reserviert den Pfad-Präfix `/.well-known/` und ist damit das Muster, an dem sich eine etwaige Pfad-Konvention für die Kaskade orientieren würde.

**Quelle 6 · GraphQL: Introspection.** https://graphql.org/learn/introspection/ — *Wofür sie hier steht:* Native Selbstbeschreibung der Mechanik in einem anderen Dialekt, samt Beschreibungsfeldern am Schema — die Anschlussstelle für die Analogie aus F6 und Gegenstand von Q5. Sie ist zugleich die Schnittstellenmechanik der Wiki-Plattform aus §8.

**Quelle 7 · gRPC: Reflection.** https://grpc.io/docs/guides/reflection/ — *Wofür sie hier steht:* Native Selbstbeschreibung der Mechanik bei gRPC; ein Dienst gibt seine Protobuf-Definitionen über einen standardisierten Aufruf preis. Vollständigkeit der Mechanik ohne Kuratierung — genau die Lücke, die F6 füllt; Gegenstand von Q5.

**Quelle 8 · OpenRPC.** https://open-rpc.org/ — *Wofür sie hier steht:* Der Vertragsmechanismus für JSON-RPC 2.0, den F6 als analog anzureichernden Dialekt nennt; sein `rpc.discover` ist das dortige Gegenstück zur Laufzeit-Abfrage. Gegenstand von Q5.

**Quelle 9 · Wiki.js: GraphQL-API (Entwicklerdokumentation).** https://docs.requarks.io/dev/api — *Wofür sie hier steht:* Die Plattform, auf die sich die Vorform in §8 bezieht, und der Typ von Bestands-API, für den F15 die Fassade vorsieht — eine vollständige GraphQL-Schnittstelle ohne kuratierten Einstieg für Agenten.

**Quelle 10 · Wiki-Werkzeugkasten des Autors, Repository `syncwiki`.** https://github.com/joern-schlingensiepen/syncwiki — *Wofür sie hier steht:* Beleg 2 in §8 (Vorform in Betrieb, 140.000 Knoten, Zähl-Widerspruch „12" neben „13").

**Quelle 11 · Mail-Ablage-Dienst des Autors, Repository `imap-service`.** https://github.com/schlingensiepen/imap-service — *Wofür sie hier steht:* Beleg 1 in §8 (Referenzimplementierung).

**Quelle 12 · Dissertation des Autors, 2007: Selbstbeschreibungsdienste mit Qualitätsmerkmalen in einem Peer-to-Peer-System mit Grid-Funktionen.** *[Bibliographische Angabe vom Autor nachzutragen — Titel, Hochschule, Fundstelle.]* — *Wofür sie hier steht:* Beleg 3 in §8 (Vorgeschichte) und die Herkunft der Qualitätsdimension, die §7 ausdrücklich nicht abdeckt.

**Quelle 13 · Schwester-Dossier dieser Reihe: *capability-tokens-for-agents* — in Vorbereitung, noch nicht veröffentlicht, daher ohne Fundstelle.** — *Wofür sie hier steht:* Die Vertiefung zu F12 und F13: Begründung und Handhabungs-Kodex des engen Token-Zuschnitts. Vertiefung, kein Verständnis-Muss — die hier gebrauchten Kernregeln fassen F12 und F13 eigenständig zusammen.

**Einordnung.** Die konzeptuelle Linie dieses Dossiers verläuft zwischen zwei Traditionen. Die eine ist Hypermedia (Quelle 3): Antworten, die die nächsten Schritte selbst benennen. Sie kam zu früh, weil menschliche Entwickler zur Laufzeit nichts entdecken wollten; Agenten tun es klaglos, und F7 löst das Versprechen spät ein. Die andere ist die generierte Mechanik-Selbstbeschreibung einzelner Dialekte (Quellen 1, 6, 7, 8): Sie liefert Vollständigkeit, aber keine Gewichtung — der Konsument erfährt, was es gibt, nicht, was zusammengehört und was welche Folgen hat. Dieses Dossier ergänzt an drei Stellen: Kuratierung *im* Träger statt daneben (F6), eine Verhaltensebene neben der Mechanik (F11) und einen gestuften Einstieg, der die Entscheidung vor das Laden setzt (F2). Quelle 2 zeigt dasselbe Prinzip für Tools, Quelle 4 eine schlankere Form derselben Idee ohne Laufzeit-Rendering.
