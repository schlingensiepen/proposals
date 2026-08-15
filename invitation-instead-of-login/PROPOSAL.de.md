---
type: Konzept
status: Entwurf
version: "1.0"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: invitation-instead-of-login
language: de (führend)
question: "Wie weiß der Server, wer da kommt — ohne Login-Formular, ohne Passwort und ohne Gäste?"
thesis: "Zugang entsteht durch Einladung; ein Link lädt einen Benutzer für alle seine Geräte ein, weil jedes Einlösen einen eigenen Nachweis erzeugt — und Maschinen bekommen keine Identität, sondern eine Fähigkeit."
---

# Eingeladen statt angemeldet

**In einem Satz:** Es gibt kein Login-Formular und keine Registrierung — wer
hereinkommt, wurde eingeladen und tauscht den Einladungslink **auf jedem Gerät
einmal** gegen einen eigenen Nachweis; Maschinen bekommen davon getrennt
Schlüssel, die eine Aufgabe erlauben und keine Person darstellen.

**Kurzthese:** Ein Passwort ist ein Geheimnis, das ein Mensch verwahren muss,
und ein Login-Formular ist die Stelle, an der man es erraten kann. Beides
entfällt, wenn Zugang ein **Vorgang** ist statt einer Eigenschaft: Jemand lädt
ein, der Eingeladene löst ein, und ab da weist ihn sein Gerät aus. **Ein
einziger Link genügt für alle Geräte einer Person** — er wird auf jedem einmal
eingelöst und erzeugt dort einen eigenen Nachweis, der einzeln entzogen werden
kann. Der Preis ist, dass derselbe Link auch in fremder Hand wirkt.

## 1. Problem

„Ich baue etwas für einen kleinen, bekannten Kreis — Familie, Verein, ein
Team. Ich will keine Registrierung, keine Passwörter, kein Zurücksetzen per
Mail, keinen zweiten Faktor. Aber ich will auch nicht, dass jeder mitliest, der
die Adresse kennt. Was ich brauche, ist: Ich sage, wer dazugehört, und danach
funktioniert es einfach."

Darin stecken drei Fragen, die getrennt beantwortet werden müssen:

- **Wie kommt jemand zum ersten Mal herein**, ohne dass er sich etwas ausdenken
  und merken muss?
- **Woran erkennt der Server ihn danach**, ohne dass er bei jedem Besuch etwas
  eingibt?
- **Und was ist mit den Maschinen?** Ein Sicherungsprozess, ein Auswertungsdienst
  — die haben kein Gerät, keine Person und keine Mailadresse, brauchen aber
  Zugriff.

Die dritte Frage wird in dieser Lage fast immer falsch beantwortet: Der
Hintergrundprozess bekommt ein Benutzerkonto. Damit ist eine Identität
entstanden, die niemandem gehört, unbefristet gilt und dieselben Rechte hat wie
ein Mensch.

## 2. Das Konzept

**Es gibt keine Gäste.** Jeder Inhalt setzt einen Nachweis voraus — auch
Lesen, auch mit einer schwer erratbaren Adresse in der Hand. Wer dazugehören
soll, bekommt einen **Einladungslink**. Er trägt einen Token, der **nichts
weiter berechtigt**, als eingetauscht zu werden — und das **beliebig oft**: Beim
Öffnen liest die Anwendung ihn aus, schickt ihn, und der Server antwortet mit
einem Nachweis, den er selbst setzt. **Ein Link genügt damit für alle Geräte
einer Person**; auf jedem wird er einmal eingelöst und hinterlässt dort einen
eigenen Nachweis.

Dieser Nachweis gehört **einem Gerät**, nicht einer Person. Der Datensatz
dahinter ist ein „Benutzer auf Gerät": Dieselbe Person auf Handy und Tablet hat
zwei Nachweise, zwei Personen auf demselben Handy ebenso. Nur so lässt sich ein
verlorenes Gerät sperren, ohne alle anderen zu treffen. Ein Einladungslink ist
deshalb **mehrfach einlösbar** — jedes Einlösen erzeugt ein weiteres Gerät —,
und **jedes Gerät hält fest, aus welcher Einladung es entstanden ist**. Damit
ist beantwortbar, *welche* Geräte aus einem Link stammen, nicht nur wie viele.
Der Link selbst hat einen **Ablauf** und lässt sich **zurückziehen**; beides
wirkt nur nach vorn — was er bereits erzeugt hat, wird einzeln entzogen.

**Rechte hängen am Benutzer, nicht am Nachweis.** Der Server schlägt sie bei
jedem Zugriff nach, statt sie in den Nachweis zu schreiben; ein Entzug wirkt
damit sofort und nicht erst mit dem Ablauf eines Cookies.

**Maschinen bekommen etwas anderes.** Kein Benutzerkonto, sondern einen
Schlüssel, der **eine Aufgabe** erlaubt — sichern, auswerten — und keine Person
darstellt. Solche Schlüssel entstehen nicht im laufenden Betrieb, sondern nur
während eines **Bootstrap-Zeitfensters**, das ein Deployment öffnet.

## 3. Begriffe

- **Nachweis** — eine Angabe, mit der ein Aufrufer belegt, wer oder was er ist.
- **Identitätsnachweis** — ein Nachweis, der auf einen **Träger** verweist
  (diesen Menschen auf diesem Gerät); was er darf, wird nachgeschlagen.
- **Fähigkeitsschlüssel** — ein Nachweis, der eine **Aufgabe** erlaubt, ohne
  jemanden darzustellen; was er darf, steht in ihm selbst.
- **Einladung** — ein Vorgang, der einen Token erzeugt, dessen einziger Zweck es
  ist, gegen einen Identitätsnachweis getauscht zu werden.
- **Einlösen** — der Tausch: Einladungstoken hinein, Identitätsnachweis heraus.
  **Je Gerät einmal, je Link beliebig oft** — die beiden „einmal" nicht
  verwechseln: Ein Gerät braucht den Tausch genau einmal, ein Link erlaubt ihn
  ohne Begrenzung.
- **Gerät** — der Datensatz, an dem ein Identitätsnachweis hängt; genauer:
  „Benutzer auf Gerät". Er hält fest, aus welcher Einladung er entstanden ist.
- **Entziehen** — einen einzelnen Nachweis unwirksam machen, ohne andere zu
  treffen. Abzugrenzen vom **Zurückziehen** einer Einladung, das nur künftige
  Einlösungen verhindert.
- **Bootstrap-Fenster** — ein befristeter Zustand, in dem Vorgänge erlaubt sind,
  die im laufenden Betrieb verboten sind.
- **Fragment** — der Teil einer Adresse hinter dem `#`. Der Browser schickt ihn
  **nicht an den Server**; er steht in keinem Zugriffslog und in keinem
  `Referer`.

## 4. Festlegungen

### F1 · Es gibt keine Gäste

**Was gilt.** Jeder Zugriff auf Inhalte verlangt einen Nachweis — Lesen
eingeschlossen. Es gibt keine Vorstufe, in der der Besitz einer schwer
erratbaren Adresse zum Lesen genügt.

**Warum.** Eine Lesestufe „wer die Adresse hat, darf sehen" wirkt harmlos und
ist der teuerste Teil des Systems: Sie macht die Unerratbarkeit der Adresse zur
**alleinigen** Sicherung aller Inhalte, die darüber erreichbar sind — ohne
Sperrmöglichkeit, ohne Nachvollziehbarkeit, ohne Begrenzung des Versuchens. Und
sie wächst mit: Was heute „nur die Übersicht" zeigt, zeigt nach der nächsten
Erweiterung die Historie samt der Namen aller Beteiligten.

Der Preis ist geringer, als er scheint. Die Alternative wäre bequem, wenn die
Einrichtung aufwendig wäre — sie besteht aber aus **einem Klick**, den ohnehin
jeder macht, der mehr will als eine einzelne Seite ansehen.

**Naheliegend und verworfen.** *Lesen frei, Schreiben geschützt* — die
klassische Aufteilung, und sie unterstellt, dass Lesen harmlos ist; bei
Beständen mit Historie, Namen und Standorten ist es das nicht. *Unerratbare
Adresse als Zugangssicherung* — sie ist ein Schutz gegen das Durchprobieren von
Bezeichnern, kein Ersatz für einen Nachweis; sie lässt sich nicht zurückziehen
und steht in jedem Zugriffslog, das sie je gesehen hat. *„Erst mal offen,
später absichern"* — die Absicherung kommt nie, weil sie dann Bestandsnutzer
aussperrt.

**Woran man merkt, dass es verletzt ist.** Es gibt eine Adresse, unter der man
ohne Nachweis Inhalte sieht — und sei es „nur" eine Übersicht.

### F2 · Zugang entsteht durch Einladung, nicht durch Registrierung

**Was gilt.** Es gibt kein Anmeldeformular, kein Passwort und keine
Selbstregistrierung. Wer dazugehören soll, wird von jemandem mit dem Recht dazu
**eingeladen**.

**Warum.** Ein Passwort verlagert die Sicherheit des Systems in den Kopf und den
Passwortspeicher der Benutzer — und erzeugt sofort die Folgeprobleme, die den
größeren Teil jeder Anmeldelösung ausmachen: Zurücksetzen, Mindestanforderungen,
Wiederverwendung, zweiter Faktor. Ein Anmeldeformular ist außerdem die Stelle,
an der ein Fremder Versuche unternehmen kann; ohne Formular gibt es nichts zu
erraten.

Der eigentliche Gewinn ist aber ein anderer: **Bei einer Einladung ist die
Zugehörigkeit eine Entscheidung, die jemand trifft**, und nicht ein Zustand, den
jemand behauptet. Für einen bekannten Kreis ist das die ehrlichere Abbildung —
es gibt niemanden, der „sich anmelden können soll", sondern nur Leute, die
dazugehören.

**Naheliegend und verworfen.** *Benutzername und Passwort* — vertraut, und man
kauft damit den gesamten Anhang: Zurücksetzen, Sperrlogik, Anforderungen an die
Wahl. *Anmeldung über einen fremden Anbieter* — erspart die Verwahrung, macht
aber den Zugang von einem Dritten abhängig und setzt voraus, dass jeder dort ein
Konto hat. *Einmal-Codes per Mail bei jedem Besuch* — löst dasselbe Problem und
verlangt bei jedem Zugriff einen Postfachwechsel; das trägt bei seltener
Nutzung, nicht bei täglicher.

**Was das von der Anwendung verlangt: zwei Vorgänge, nicht einen.** Einladen und
einen Benutzer anlegen sind **nicht dasselbe**, und wer sie zusammenlegt, merkt
es erst, wenn die erste Einladung abgelaufen ist (F3):

1. **Jemanden neu aufnehmen** — legt den Benutzer an *und* erzeugt seine erste
   Einladung.
2. **Für einen vorhandenen Benutzer eine neue Einladung erzeugen** — ohne einen
   zweiten Benutzer anzulegen. Das ist der Regelfall nach dem ersten Tag:
   abgelaufener Link, verlorenes Gerät, ein weiteres Gerät.

Ohne den zweiten Vorgang entsteht bei jeder Nachforderung **eine Dublette der
Person** — und damit sind Rechte, Verlauf und Sperrmöglichkeiten über zwei
Einträge verteilt, die niemand mehr zusammenführt. Die Selbstbedienung („neuen
Zugangslink anfordern") ist derselbe zweite Vorgang, nur vom Betroffenen
ausgelöst statt vom Einladenden.

**Woran man merkt, dass es verletzt ist.** Es existiert eine Seite, auf der
jemand ein Kennwort eingeben kann — oder dieselbe Person steht zweimal in der
Liste, weil sie einen neuen Link gebraucht hat.

### F3 · Der Einladungstoken ist nicht der Nachweis — und jedes Gerät weiß, woher es kommt

**Was gilt.** Der Token im Einladungslink berechtigt zu **genau einem**:
eingetauscht zu werden — und das **beliebig oft**. Jeder Tausch erzeugt ein
weiteres Gerät mit **eigenem** Nachweis. Der Nachweis entsteht erst dabei, wird
vom **Server** gesetzt und existiert nur dort und im Browser.

**Das ist kein hingenommener Nebeneffekt, sondern der Zweck.** Ein Mensch hat
Handy, Tablet und Rechner. Wäre der Link einmal einlösbar, bräuchte er drei
Einladungen für drei Geräte — drei Mails, drei Vorgänge, dreimal die Frage, ob
schon eingelöst wurde. So genügt **ein Link für einen Benutzer und alle seine
Geräte**, und weil jeder Tausch einen eigenen Nachweis hinterlässt, bleibt jedes
Gerät einzeln entziehbar (F4). Die Einladung ist damit eine Aussage über die
**Person** — „du gehörst dazu" —, nicht über ein Endgerät.

**Jedes Gerät hält fest, aus welcher Einladung es entstanden ist.** Die
Einladung selbst führt **keinen Zähler**. Beides zusammen ergibt die Auskunft,
auf die es ankommt: *welche* Geräte aus einem Link entstanden sind — und die
lässt sich auch benutzen.

Dazu zwei Vorgänge, die es geben muss:

| Vorgang | Wirkung |
|---|---|
| **Einladung zurückziehen** | Der Link ist nicht mehr einlösbar. Bereits entstandene Geräte bleiben **unberührt** und werden einzeln entzogen |
| **Ablauf der Einladung** (empfohlen) | Der Link verfällt von selbst, auch wenn niemand ihn zurückzieht |

**Warum.** Trüge der Link den Nachweis selbst, stünde das Sitzungsgeheimnis
dauerhaft in einer Mail — im Postfach, im Postfach-Backup, in jeder
Weiterleitung. Und alle, die denselben Link öffnen, teilten sich einen einzigen
Nachweis; Geräte wären damit nicht unterscheidbar, und genau darauf beruht F4.

**Warum die Herkunft am Gerät steht und nicht als Zahl an der Einladung.** Ein
Zähler sagt „aus diesem Link sind vier Geräte entstanden" — und lässt einen
damit stehen. Handeln kann man erst, wenn man weiß, **welche vier**: Genau dann
lassen sie sich ansehen, benennen, einzeln entziehen. Steht die Herkunft am
Gerät, sind Anzahl und letzte Einlösung ohnehin ableitbar; ein eigenes Feld
dafür wäre eine zweite Wahrheit, die auseinanderlaufen kann. (Die Spur reicht so
weit, wie die Geräte reichen: Ein **gelöschtes** Gerät nimmt sie mit — ein
Grund mehr, Entziehen zum Regelfall und Löschen zur Ausnahme zu machen.)

**Der Ablauf ist eine Empfehlung, keine Vorgabe** — aber eine mit Nachdruck. Ein
Link ohne Ablauf bleibt in einem Postfach liegen, und Postfächer überdauern
Geräte, Personen und Projekte. Ein Ablauf kostet nichts außer dem zweiten
Vorgang aus F2: Wer nach der Frist noch hereinkommen soll, bekommt eine neue
Einladung — kein neues Konto, keine Dublette. **Genau deshalb sind die beiden
Vorgänge in F2 keine Bequemlichkeit, sondern die Bedingung dafür, dass ein
Ablauf überhaupt praktikabel ist.**

**Mehrfach einlösbar ist eine bewusste Entscheidung gegen die strengere
Variante.** Ein einmaliger Link zwänge für jedes weitere Gerät derselben Person
zu einem neuen Vorgang — bei drei Geräten drei Mails. Der Sicherheitsgewinn wäre
gering, denn wer den Link hat, hat ihn ohnehin; das eigentliche Risiko ist der
weitergegebene Link, und den verhindert Einmaligkeit nur beim ersten Mal.

**Das Zusammenspiel der drei Hebel** — und dass es drei sind, ist der Punkt:
Der **Ablauf** begrenzt, wie lange ein Link überhaupt wirken kann. Das
**Zurückziehen** beendet ihn vorzeitig, wenn er in falsche Hände geraten ist.
Das **Entziehen einzelner Geräte** räumt auf, was er bereits erzeugt hat. Keiner
ersetzt einen anderen: Zurückziehen wirkt nicht rückwirkend, Entziehen nicht
vorbeugend, und ein Ablauf hilft nicht gegen den Link, der heute weitergegeben
wurde.

**Naheliegend und verworfen.** *Der Link trägt den Nachweis* — spart den Tausch
und legt das Sitzungsgeheimnis dauerhaft in ein Postfach. *Einmalig einlösbar* —
strenger, aber teuer im Alltag und wirkungslos gegen den eigentlichen Fall.
*Einen Zähler an der Einladung führen statt der Herkunft am Gerät* — beantwortet
„wie viele", nicht „welche"; man weiß dann, dass etwas nicht stimmt, kann aber
nichts tun. *Auf den Ablauf verzichten, „die Einladung wird ja benutzt"* — sie
wird einmal benutzt und bleibt danach jahrelang gültig in einem Postfach liegen.

**Woran man merkt, dass es verletzt ist.** Ein Sitzungsgeheimnis lässt sich aus
einer Mail herauskopieren — oder man sieht, dass aus einer Einladung mehr Geräte
entstanden sind als erwartet, und kann nicht sagen, welche es sind.

### F4 · Ein Nachweis je Gerät — der Datensatz ist „Benutzer auf Gerät"

**Was gilt.** Jedes Gerät bekommt einen eigenen Nachweis. Der Datensatz
dahinter beschreibt nicht eine Person und nicht ein Gerät, sondern die
Verbindung: **dieser Benutzer auf diesem Gerät**. Die Person trägt Name, Adresse
und Rechte; das Gerät trägt den Nachweis und seine Bezeichnung.

**Warum.** Was man nicht auseinanderhalten kann, kann man nicht einzeln sperren
— und einzeln sperren ist der einzige Vorgang, der bei einem verlorenen Gerät
zählt. Ein gemeinsamer Nachweis für alle Geräte einer Person zwingt im Ernstfall
dazu, die Person auszusperren statt das Gerät.

Die Trennung ist auch die einzige, die den Alltag richtig abbildet: Eine Person
hat mehrere Geräte, und ein Gerät wird von mehreren Personen benutzt. Erst
„Benutzer auf Gerät" trägt beides. Nebenbei wird damit eine Auskunft möglich,
die sonst fehlt: Der Verlauf kann festhalten, **wer** etwas getan hat **und von
welchem Gerät** — bei einem verlorenen Gerät die entscheidende Frage.

**Wo der Nachweis liegt: in einem Cookie, das der Server setzt.** Nicht in einem
Speicher, den die Anwendung selbst verwaltet — und das hat **zwei** Gründe, von
denen meist nur der erste genannt wird:

1. **Der Client kann es nicht lesen** (`HttpOnly`). Eine Schwachstelle im
   Browsercode kommt nicht daran, und es gerät nicht versehentlich in eine
   Protokollierung.
2. **Es überlebt.** Moderne Browser räumen Speicher, den **Skriptcode** angelegt
   hat, nach wenigen Tagen ohne Nutzung weg — Tracking-Schutz macht keinen
   Unterschied zwischen Verfolgung und Anmeldung. Ein Cookie, das der Server per
   Antwortkopfzeile setzt, ist davon ausgenommen und behält seine Laufzeit. Wer
   den Nachweis im Skriptspeicher ablegt, baut ein Modell, das nach einer Woche
   Urlaub jeden aussperrt.

Dazu drei Werte und eine Falle, die man kennen muss:

| | |
|---|---|
| Laufzeit | lang, aber **nach oben begrenzt** — Browser deckeln sie auf rund 400 Tage, gleich welcher Wert gesendet wird. Nach gut einem Jahr ist also ohnehin eine neue Einlösung nötig |
| Erneuerung | bei jedem Besuch, damit ein täglich benutztes Gerät nie mitten im Gebrauch herausfällt |
| Beigaben | `Secure` und eine Weitergaberegel, die den Nachweis bei fremdinitiierten Aufrufen zurückhält |
| **Falle** | Zeigt die eigene Adresse per Aliasnamen auf einen **fremden** Host, behandeln manche Browser auch server-gesetzte Cookies wie Skriptspeicher — und kappen sie auf wenige Tage. Die Langlebigkeit hängt damit an der Namensauflösung, nicht nur am Code |

**„Gerät" heißt in Wahrheit „Browserprofil" — und das muss man aussprechen.** Ein
Nachweis lebt in einem Cookie-Speicher, und davon hat ein Rechner mehrere: Wer
denselben Link in zwei Browsern öffnet, bekommt **zwei** Einträge für **ein**
Stück Blech. Dasselbe gilt für getrennte Profile, private Fenster und
Arbeitsprofile. Das ist kein Fehler des Modells — es zählt Zugänge, nicht
Hardware —, aber es überrascht jeden, der die Liste zum ersten Mal ansieht.

**Deshalb die Empfehlung: den vollständigen Kennungsstring des Browsers
aufheben**, den er bei jedem Aufruf ohnehin mitschickt, sowie die zuletzt
gesehene Netzadresse — **jeweils nur den letzten Wert, keine Historie**. Er ist
**kein Nachweis** und keine Identität; er ist eine Wiedererkennungshilfe für den
Menschen, der vor der Liste sitzt und entscheiden muss, welcher der beiden
Einträge „Rechner Büro" das Firefox-Profil war. Ohne ihn steht dort zweimal
derselbe Name, und niemand traut sich zu entziehen.

**Die Bezeichnung vergibt der Anwender beim Einlösen**, nicht der Einladende. Er
weiß besser, welches Gerät er in der Hand hält; eine vom Admin geratene
Bezeichnung ist nach dem zweiten Gerät wertlos. Sie bleibt **maßgeblich** — der
Kennungsstring hilft beim Wiedererkennen, entscheidet aber nichts.

**Naheliegend und verworfen.** *Ein Nachweis je Person* — einfacher, und er
macht das Sperren eines Geräts unmöglich. *Ein Nachweis je Gerät ohne Person
dahinter* — dann hat niemand eine Adresse, über die er sich einen neuen holen
kann, und dieselbe Person erscheint mehrfach ohne erkennbaren Zusammenhang.
*Geräte automatisch erkennen* statt sie zu benennen — technische Merkmale sind
unzuverlässig und ändern sich mit jedem Update; sie taugen als Gedächtnisstütze,
nicht als Kennung. *Den Nachweis im Skriptspeicher halten* — sieht aufgeräumter
aus, verliert `HttpOnly` **und** die Langlebigkeit. *Mehrere Browser desselben
Rechners zusammenfassen* — es gibt kein verlässliches Merkmal dafür, und der
Versuch verwechselt Zugänge mit Hardware; genau die Unterscheidung, die beim
Entziehen zählt.

**Woran man merkt, dass es verletzt ist.** Ein verlorenes Gerät lässt sich nur
sperren, indem man jemanden aussperrt.

### F5 · Autorisiert wird gegen den Bestand, nicht gegen den Nachweis

**Was gilt.** Der Nachweis belegt **wer**, nicht **was jemand darf**. Rechte
stehen beim Benutzer und werden bei jedem Zugriff nachgeschlagen; sie werden
nicht in den Nachweis geschrieben.

**Was im Nachweis steht: nichts als ein Verweis.** Er ist ein Zufallswert ohne
Aussage — keine Rolle, kein Name, kein Rechtebündel, kein eingebautes
Ablaufdatum. Gespeichert ist er nur als **Streuwert**: Wer den Bestand erbeutet,
bekommt keine funktionierenden Nachweise in die Hand.

**Was bei jedem Zugriff geschieht** — vier Schritte, und ihre Reihenfolge ist
Teil der Festlegung:

1. **Nachweis auflösen** — Streuwert bilden, Gerät dazu suchen. Nichts gefunden:
   nicht angemeldet.
2. **Gerät prüfen** — ist es entzogen, endet es hier, egal wer dahintersteht.
3. **Benutzer prüfen** — ist er stillgelegt, endet es hier, und zwar für **alle**
   seine Geräte auf einmal.
4. **Recht prüfen** — erst jetzt, und **vor** jeder Auflösung fachlicher
   Bezeichner. Andernfalls verrät schon die Fehlermeldung, ob ein Objekt
   existiert.

**Warum.** Ein Recht, das im Nachweis steht, gilt
bis zu dessen Ablauf. Bei Nachweisen, die über Monate gültig bleiben — und das
ist hier der Regelfall —, heißt das: Ein Entzug wirkt Monate später.
Nachgeschlagene Rechte wirken beim **nächsten Zugriff**, in beide Richtungen:
Entzug **und** Erteilung, auf allen Geräten der Person, ohne dass sich jemand
neu anmelden muss.

Aus derselben Eigenschaft folgen zwei Vorgänge, die es sonst nicht gäbe: Ein
**Gerät** entziehen wirkt sofort, weil Schritt 2 es findet. Einen **Benutzer**
stilllegen wirkt sofort auf all seine Geräte, weil Schritt 3 dort greift — ohne
dass man die Geräte einzeln aufsuchen müsste.

**Der Nachweis wird bei jedem Besuch erneuert.** Nur deshalb darf er langlebig
sein, ohne dauerhaft zu sein: Ein Gerät, das ein Jahr nicht benutzt wurde, fällt
von selbst heraus, ein täglich benutztes läuft nie mitten im Gebrauch ab.

**Der Preis ist ein Nachschlagen je Zugriff.** Bei einem System, das den Bestand
ohnehin bei jedem Zugriff öffnet, ist er nicht messbar; er wäre es bei einer
Architektur, die genau das vermeiden will. **Wer ihn zwischenspeichert, kauft
sich die Verzögerung zurück, die er vermeiden wollte** — und zwar in genau der
Höhe der Zwischenspeicherung. Das ist zulässig, muss dann aber ausgesprochen
werden: „Entzug wirkt binnen fünf Minuten" ist eine Zusage, „Entzug wirkt
sofort" wäre dann falsch.

**Naheliegend und verworfen.** *Rechte in einen signierten Nachweis schreiben* —
spart das Nachschlagen und ist der Grund, warum Sperrlisten erfunden werden
mussten; die Sperrliste ist dann das Nachschlagen, nur umständlicher und mit
einer zweiten Wahrheit daneben. *Kurze Laufzeiten statt Nachschlagen* — die
übliche Antwort darauf; sie tauscht die Verzögerung beim Entzug gegen einen
Erneuerungsvorgang, der selbst ausfallen kann, und passt schlecht zu Geräten,
die wochenlang nicht benutzt werden. *Ein zweiter Nachweis für erhöhte Rechte*
(„Verwalten nur am Rechner zu Hause") — verdoppelt den Zustand, den man
widerspruchsfrei halten muss, für einen Schutz, den jemand mit Gerätezugriff
ohnehin aushebelt. *Den Nachweis im Klartext ablegen, „ist ja nur eine
Sitzung"* — ein erbeuteter Bestand ist dann eine Sammlung gültiger Zugänge.

**Woran man merkt, dass es verletzt ist.** Nach dem Entzug eines Rechts
funktioniert es auf einem Gerät weiter.

### F6 · Menschen bekommen Identität, Maschinen bekommen Fähigkeit

**Was gilt.** Ein Mensch bekommt einen Nachweis, der auf ihn verweist; was er
darf, wird nachgeschlagen (F5). Eine Maschine bekommt einen Schlüssel, der
**eine Aufgabe** erlaubt — sichern, auswerten — und **niemanden darstellt**. Je
Aufgabe ein eigener Schlüssel, je Maschine ein eigener.

**Warum.** Der Unterschied ist nicht kosmetisch. Ein Identitätsnachweis fragt
„wer bist du?" und schlägt danach nach; ein Fähigkeitsschlüssel trägt seine
Erlaubnis in sich und fragt niemanden. Das passt zu dem, was die beiden
tatsächlich sind: Ein Mensch wechselt Geräte, bekommt und verliert Rechte, hat
eine Adresse, unter der man ihn erreicht. Ein Hintergrundprozess hat nichts
davon — er hat genau eine Aufgabe und läuft, bis jemand ihn abschaltet.

Gibt man einer Maschine ein Benutzerkonto, entsteht eine **Identität, die
niemandem gehört**: unbefristet, ohne Adresse, ohne jemanden, der sie vermisst,
wenn sie missbraucht wird — und mit allen Rechten, die man ihr der Bequemlichkeit
halber gegeben hat. Umgekehrt ist ein Fähigkeitsschlüssel für einen Menschen
falsch, weil man ihm nicht ansieht, wer ihn benutzt hat.

**Die Fähigkeitsseite in ihrer strengen Form heißt: ein Schlüssel, eine
Fähigkeit, ein Ort.** Kein Parameter, mit dem der Aufrufer seinen Wirkungsbereich
wählt — der Schlüssel **ist** der Wirkungsbereich. Wer zwei Aufgaben hat,
bekommt zwei Schlüssel; wer an zwei Orten arbeitet, ebenso. Damit gibt es keinen
Autorisierungscode, der falsch sein könnte, und ein Entzug trifft genau eine
Fähigkeit an genau einem Ort. Wie ein solcher Schlüssel darüber hinaus zu
handhaben ist — Übergabe, Ablage, Rotation, Verfall —, ist ein Gegenstand für
sich; Abschnitt 10 nennt ihn.

**Je Aufgabe und je Maschine getrennt**, weil sonst der Schadensfall alles
kostet: Der Sicherungszugang gibt den **gesamten** Bestand auf einmal heraus;
der Auswertungszugang sieht nur, was er gerade bearbeitet. Diese beiden in einem
Schlüssel zusammenzufassen spart eine Zeile Konfiguration und kostet im
Schadensfall den Unterschied zwischen beidem. Und der Name des Schlüssels trägt
seine Maschine, damit ein übernommener Rechner **einzeln** stillgelegt werden
kann — dasselbe Argument wie in F4.

**Naheliegend und verworfen.** *Ein technischer Benutzer für den Dienst* — der
verbreitetste Weg, und er erzeugt genau die herrenlose Identität von oben.
*Ein Schlüssel für alle Maschinenaufgaben* — spart Konfiguration und kostet im
Schadensfall alles. *Menschen ebenfalls über Fähigkeitsschlüssel* — dann ist
nicht mehr feststellbar, wer etwas getan hat, und der Verlauf wird wertlos.

**Woran man merkt, dass es verletzt ist.** In der Benutzerliste steht ein
Eintrag, der niemandem gehört — oder ein einzelner Schlüssel öffnet sowohl die
Auswertung als auch den gesamten Bestand.

### F7 · Maschinenzugänge entstehen nur im Bootstrap-Fenster

**Was gilt.** Ein Fähigkeitsschlüssel kann **nicht** im laufenden Betrieb
erzeugt werden, auch nicht von einem Verwalter. Er entsteht ausschließlich
während eines offenen Bootstrap-Fensters (F8). Sein Klartext erscheint dabei
**genau einmal**; danach liegt im System nur noch ein Streuwert.

**Das Fenster geht bei jedem Deployment auf, nicht nur beim ersten.** Damit ist
die Festlegung im Alltag keine Einschränkung, sondern eine Terminierung: Wer
eine neue Maschine anschließt, liefert aus und lässt in demselben Zug ihre
Schlüssel erzeugen — der Einrichtende hält das Setup-Geheimnis ohnehin in der
Hand, solange er deployt. Es gibt keinen Fall „ich brauche jetzt einen
Schlüssel und komme nicht heran"; es gibt nur den Fall „ich brauche einen
Schlüssel, also deploye ich".

**Warum.** Ein Verwaltungsnachweis liegt monatelang auf einem Gerät. Könnte man
damit einen Maschinenschlüssel ausstellen, wäre ein einziges erbeutetes Gerät
genug, um sich einen **dauerhaften** Zugang zu verschaffen — einen, der die
Sperrung des Geräts überlebt und im Zweifel den gesamten Bestand herausgibt. Die
Erzeugung dauerhafter Maschinenzugänge gehört deshalb hinter dieselbe Schwelle
wie der Bootstrap selbst.

Der zweite Grund ist eine Haltung, die sich auszahlt: **Ein Maschinenzugang ist
ein bewusster Akt, kein Nebenprodukt.** Ein Verwalter, der von Hand sichern
will, erzeugt sich dafür einen eigenen Schlüssel — statt dass ein solcher Zugang
als stilles Nebenrecht in einer Rolle mitläuft.

**Erzeugen ist befristet — Entziehen ist jederzeit.** Diese Asymmetrie ist
Absicht und muss ausgesprochen werden, weil sie sich nicht von selbst ergibt:
Ein Fähigkeitsschlüssel lässt sich **im laufenden Betrieb stilllegen**, ohne
Fenster, ohne Deployment, ohne Setup-Geheimnis — ein Verwalter genügt. Andernfalls
liefe ein übernommener Rechner bis zum nächsten Deployment weiter, und die
Sperre käme genau dann zu spät, wenn sie gebraucht wird.

Die Begründung ist dieselbe wie bei Geräten (F4): Ein Vorgang, der Rechte
**wegnimmt**, darf nie schwerer erreichbar sein als der, der sie gibt. Beim
Erzeugen darf man Hürden bauen; beim Entziehen kostet jede Hürde Zeit, in der
der Schaden weiterläuft. Deshalb trägt jeder Schlüssel den Namen seiner Maschine
und ihrer Aufgabe (F6) — sonst weiß im Ernstfall niemand, welchen von fünf
Schlüsseln er stilllegen muss.

**Naheliegend und verworfen.** *Erzeugung im Verwaltungsbereich lassen* —
bequem, und sie macht einen erbeuteten Verwaltungsnachweis zum Generalschlüssel
mit unbegrenzter Haltbarkeit. *Auch das Entziehen ans Fenster binden, „der
Symmetrie halber"* — klingt konsequent und ist der Fehler: Ein kompromittierter
Rechner liefe dann bis zum nächsten Deployment weiter. *Den Klartext speichern, „damit man ihn nochmal
ansehen kann"* — dann ist er ein dauerhaft lesbares Geheimnis, und die
Einmaligkeit war umsonst.

**Woran man merkt, dass es verletzt ist.** Ein Maschinenschlüssel lässt sich
nachträglich im Klartext anzeigen — oder aus der laufenden Anwendung heraus
erzeugen.

### F8 · Der Bootstrap ist ein Zeitfenster, keine Löschdatei

**Was gilt.** Vorgänge, die es nur bei der Einrichtung geben darf — den ersten
Verwalter anlegen, Maschinenschlüssel erzeugen, das Schema wandeln —, verlangen
**zweierlei**: das stärkste Geheimnis des Systems, das außerhalb des Bestands
liegt, **und** ein Zeitfenster, das ein Deployment öffnet. Danach antworten
dieselben Vorgänge mit einer Auskunft, dass das Fenster zu ist.

**Warum.** Ein frisches System hat niemanden, der etwas erlauben könnte — es
muss sich selbst freischalten können, ohne dabei dauerhaft offen zu stehen. Das
Zeitfenster löst das, ohne ein zweites Geheimnis zu erfinden: Wer deployen kann,
hat ohnehin Zugriff auf die Konfiguration; er kann Rechte nicht erlangen, die er
nicht schon hätte — der Bootstrap macht sie nur **befristet bequem erreichbar**.

**Warum das Geheimnis außerhalb des Bestands liegen muss:** Es wird genau dann
gebraucht, wenn der Bestand leer, beschädigt oder gerade in Wandlung ist. Läge
es darin, wäre es in dem Moment nicht erreichbar, in dem man es braucht.

**Wie das Fenster praktisch entsteht** — *Beispiel, bewusst schlicht:* Das
Auslieferungsskript legt bei jedem Lauf neben der Konfiguration eine kleine
Datei an, die zwei Dinge enthält: das **Setup-Geheimnis** und einen
**Ablaufzeitpunkt** (etwa eine Stunde später). Die Setup-Vorgänge prüfen beides;
ist der Zeitpunkt überschritten, antworten sie, dass das Fenster zu ist. Es
braucht keinen Zeitgeber, keinen Hintergrundprozess und keinen Zustand in der
Datenbank — nur einen Vergleich mit der Uhr.

**Und hier liegt die eigentliche Rechtfertigung des ganzen Verfahrens:** Wer
diese Datei schreiben kann, **hat ohnehin Zugriff auf den Rechner**. Er könnte
die Anwendung austauschen, den Bestand lesen, alles tun. Der Bootstrap gibt ihm
also keine Rechte, die er nicht schon hätte — er macht sie nur **befristet
bequem erreichbar**. Genau deshalb ist es kein Widerspruch, dass ein Fenster
existiert, in dem mehr geht als sonst: Die Vertrauensgrenze verläuft nicht am
Fenster, sondern am Schreibzugriff auf den Rechner, und die war schon vorher
dort.

**Naheliegend und verworfen.** *Eine Setup-Datei, die man nach der Einrichtung
löscht* — der klassische Weg, und er verlässt sich darauf, dass jemand daran
denkt; vergessen wird sie in genau den Fällen, in denen es darauf ankäme. *Ein
eigenes Setup-Geheimnis* — eine weitere Stelle, an der etwas auseinanderlaufen
kann, ohne Gewinn gegenüber dem, das ohnehin existiert. *Das Fenster von Hand
öffnen und schließen* — dann bleibt es offen.

**Woran man merkt, dass es verletzt ist.** Es gibt eine Datei, deren Vorhandensein
darüber entscheidet, ob jemand das System übernehmen kann.

### F9 · Geheimnisse in Adressen stehen im Fragment — oder nirgends

**Was gilt.** Muss ein Geheimnis über eine **Adresse** zum Client — ein
Einladungstoken, ein Einstiegsschlüssel —, steht es hinter dem `#`. Nicht im
Pfad, nicht im Query-String. Von dort liest die Anwendung es aus und schickt es
**einmal** im Rumpf eines Aufrufs; danach steht es in keiner Adresse mehr.

**Warum.** Alles vor dem `#` schickt der Browser an den Server, und der schreibt
es ins Zugriffslog. Es steht damit außerdem im Browserverlauf und in jedem
`Referer`, den der Browser an fremde Seiten mitschickt. Ein Geheimnis dort ist
nicht kompromittiert, wenn jemand angreift, sondern **von Anfang an**, durch den
Normalbetrieb. Das Fragment schickt der Browser **nicht** — es ist die einzige
Stelle einer Adresse, an der ein Geheimnis stehen darf.

Der Nachweis selbst reist danach nicht in der Adresse, sondern als Cookie, das
der **Server** setzt. Dass er es setzt und nicht das JavaScript, ist kein
Detail: Ein Cookie, das der Client nicht lesen kann, fließt auch bei einer
Schwachstelle im Client nicht ab und gerät nicht versehentlich in eine
Protokollierung.

**Naheliegend und verworfen.** *Token im Query-String, „ist doch nur ein Link"*
— der naheliegendste Weg und der schlechteste; er verteilt das Geheimnis auf
drei Protokolle, bevor irgendjemand etwas falsch gemacht hat. *Token im Pfad* —
dasselbe, plus `Referer`. *Nachweis im Client verwalten und als Kopfzeile
schicken* — sieht sauberer aus und verliert den Schutz davor, dass der Client
ihn preisgibt.

**Woran man merkt, dass es verletzt ist.** Im Zugriffslog steht eine Zeile, mit
der man hereinkommt.

### F10 · Das Modell stellt zwei Anker bereit — die Fachrechte hängt die Anwendung daran

**Was gilt.** Dieses Modell schreibt **keine Rechte vor**. Es ist der Unterbau
einer Fachanwendung, und was ein Eingeladener darf, weiß nur die. Bereitgestellt
werden **zwei Anker**, an die sich Rechte hängen lassen:

| Anker | trägt |
|---|---|
| **Benutzer** | alles, was der **Person** gilt: Fachrechte, Zugehörigkeiten, Mailadresse |
| **Benutzer auf Gerät** | alles, was dem **einzelnen Zugang** gilt: Bezeichnung, Herkunft, Entzug, letzte Nutzung |

An diese Anker knüpft die Anwendung ihr eigenes Rechtesystem. Dieses Dossier
verlangt davon **fünf Eigenschaften** — nicht, weil sie fachlich geboten wären,
sondern weil das Zugangsmodell ohne sie nicht trägt:

1. **Die Vorgänge dieses Modells sind selbst rechtepflichtig.** Einladen,
   Einladung zurückziehen, Gerät entziehen, Benutzer stilllegen — jeder davon
   braucht ein Recht. Ohne das darf jeder Eingeladene weiter einladen, und
   Zugehörigkeit ist keine Entscheidung mehr, sondern eine Kettenreaktion (F2).
2. **Fachrechte hängen am Benutzer, nicht am Gerät.** Sonst verschwindet ein
   Recht mit dem Entzug eines Geräts — und bei drei Geräten stellt sich die
   sinnlose Frage, welches „das mit dem Recht" ist.
3. **Sein eigenes Gerät entziehen darf jeder, ohne besonderes Recht.** Das ist
   das Abmelden. Es ist der einzige Vorgang, den ein Betroffener **selbst**
   auslösen kann, wenn ein Gerät abhandengekommen und niemand erreichbar ist —
   ohne ihn muss man im schlimmsten Moment jemanden suchen.
4. **Entziehen ist nie schwerer erreichbar als Erteilen.** Dieselbe Asymmetrie
   wie bei den Fähigkeitsschlüsseln (F7): Beim Vergeben darf man Hürden bauen,
   beim Wegnehmen kostet jede Hürde Zeit, in der der Schaden weiterläuft.
5. **Der letzte Verwalter kann sich das Verwaltungsrecht nicht selbst nehmen.**
   Sonst sperrt sich das System aus, und der einzige Weg zurück führt über den
   Bootstrap — also über ein Deployment (F8). Die Regel ist billig, ihr Fehlen
   ist teuer.

**Warum.** Ein Zugangsmodell, das auch noch die Rechte vorschreibt, ist in einer
Fachanwendung fast immer falsch: Was jemand darf, folgt aus der Sache, nicht aus
der Art, wie er hereinkam. Umgekehrt darf das Modell die Frage nicht **ganz**
offenlassen, denn vier seiner eigenen Vorgänge — einladen, zurückziehen,
entziehen, stilllegen — existieren nur durch dieses Modell und stünden sonst
ohne Zuständigkeit da. Die Trennlinie ist deshalb: **Der Zuschnitt der
Fachrechte ist offen, die Zuständigkeit für die Vorgänge des Modells ist es
nicht.**

Die fünf Eigenschaften sind keine Rechte, sondern **Aussagen über jedes
Rechtesystem**, das hier aufsetzt. Sie lassen sich mit zwei Rollen erfüllen
ebenso wie mit fünfzig.

**Naheliegend und verworfen.** *Ein Rechtemodell mitliefern* („Leser, Schreiber,
Verwalter") — bequem und in der zweiten Fachanwendung falsch; man erbt dann
Rollen, die zur Sache nicht passen, und baut die eigenen daneben. *Die Frage
ganz offenlassen* — dann hat niemand die Zuständigkeit für das Entziehen, und
sie wird beim ersten Vorfall improvisiert. *Einladen als „kleines" Recht neben
dem Verwalten führen* — wer einladen darf, vergibt Zugang zum gesamten Bestand;
es ist damit so mächtig wie das Verwalten, und es getrennt zu vergeben verlangt
eine Begründung, was ein Einladender **nicht** darf. *Rechte am Gerät statt am
Benutzer* — sieht nach Feinsteuerung aus („verwalten nur am Rechner zu Hause")
und verdoppelt den Zustand, den man widerspruchsfrei halten muss (F5).

**Woran man merkt, dass es verletzt ist.** Jemand hat sein Gerät verloren und
muss warten, bis ihn jemand anderes entziehen kann — oder ein Recht ist mit
einem entzogenen Gerät verschwunden.

**Tragwerk.** **F1 ist die Voraussetzung von allem** — ohne „keine Gäste" ist der
Rest eine Bequemlichkeit statt eines Zugriffsmodells. **F3 und F4 gehören
zusammen:** Der Tausch existiert, damit je Gerät ein eigener Nachweis entstehen
kann; ohne F4 wäre F3 überflüssig, ohne F3 wäre F4 nicht erreichbar. **F5 trägt
die Verwaltbarkeit** und ist einzeln übernehmbar — er gilt für jedes
Zugriffsmodell mit langlebigen Nachweisen. **F6 ist die Grenze zwischen den
beiden Welten**, und **F7 und F8 sind zusammen ein Bauteil, nicht zwei Regeln**:
F7 sagt, wo Fähigkeitsschlüssel **nicht** entstehen dürfen, F8 stellt den Ort
bereit, an dem sie entstehen. Beides zusammen ergibt einen praktikablen Ablauf —
ausliefern, im offenen Fenster erzeugen, Fenster schließt von selbst.

*Für Übernehmende:* Wer F7 ohne F8 mitnimmt, hat ein Verbot ohne Ersatzort. Der
Bedarf verschwindet dadurch nicht — die Sicherung braucht ihren Schlüssel —,
also rutscht die Erzeugung zurück in den laufenden Betrieb, und zwar
stillschweigend. Das ist kein Fehler dieses Modells, sondern die Art, wie es
beim Übernehmen kaputtgeht. **F9 ist
Transportsache** und unabhängig von allem anderen. **F10 hängt an F2 und F4**:
Es benennt die Zuständigkeit für die Vorgänge, die diese beiden einführen — ohne
F10 gibt es sie, aber niemanden, der sie ausführen darf. **Wer nur eine Regel
mitnimmt, nimmt F6** — sie verhindert den Fehler, der in dieser Lage am
häufigsten gemacht wird.

## 5. Sicherheitsbetrachtung

| Gefahr | Typischer Hergang | Gegenmaßnahme |
|---|---|---|
| **Weitergegebener Einladungslink** | Der Link wird weitergeleitet oder bleibt in einem Postfach, auf das jemand anders Zugriff bekommt. Er ist mehrfach einlösbar, also entstehen weitere Geräte | Drei Hebel, die einander nicht ersetzen (F3): der **Ablauf** begrenzt vorbeugend, das **Zurückziehen** beendet vorzeitig, das **Entziehen einzelner Geräte** räumt auf. Weil jedes Gerät seine Einladung kennt, ist dabei sichtbar, **welche** Geräte betroffen sind — nicht nur wie viele |
| **Alter Link in einem alten Postfach** | Eine Einladung von vor zwei Jahren wird gefunden und eingelöst. Niemand erinnert sich an sie, niemand hat sie zurückgezogen | Der empfohlene **Ablauf** (F3). Er ist der einzige Hebel, der ohne Aufmerksamkeit wirkt — Zurückziehen setzt voraus, dass jemand an den Link denkt |
| **Verlorenes Gerät** | Ein Handy mit gültigem Nachweis geht verloren. Der Nachweis gilt monatelang | Einzelsperrung, ermöglicht durch F4. Der Verlauf sagt zusätzlich, **was** von diesem Gerät aus geschehen ist — ohne F4 wäre beides unmöglich |
| **Erbeuteter Verwaltungszugang wird verewigt** | Wer ein Verwalter-Gerät übernimmt, stellt sich einen Maschinenschlüssel aus. Der überlebt die Sperrung des Geräts und gibt den gesamten Bestand heraus | F7: Maschinenschlüssel entstehen nur im Bootstrap-Fenster. Das ist der Grund für diese Festlegung, nicht ein Nebeneffekt |
| **Herrenlose Identität** | Ein Hintergrundprozess bekommt ein Benutzerkonto „wie ein Mitarbeiter". Es hat keine Adresse, keinen Besitzer, kein Ablaufdatum — und niemand vermisst es | F6: Maschinen bekommen Fähigkeit, keine Identität. Ein Konto ohne Menschen dahinter fällt in keiner Prüfung auf, weil es aussieht wie alle anderen |
| **Ein Schlüssel für alles** | Sicherung und Auswertung teilen sich einen Schlüssel, „ist ja derselbe Dienst". Der Auswertungsrechner steht im Wohnzimmer | Trennung je Aufgabe **und** je Maschine (F6). Der Sicherungszugang ist der einzige, der alles auf einmal herausgibt |
| **Geheimnis im Zugriffslog** | Ein Einladungstoken steht im Query-String. Er landet im Serverlog, im Browserverlauf und im `Referer` | F9: Fragment oder nirgends. Diese Gefahr entsteht ohne Angriff, allein durch Normalbetrieb |
| **Recht entzogen, wirkt nicht** | Ein Verwalterrecht wird entzogen, der Nachweis auf dem Gerät gilt aber weiter | F5: Rechte werden nachgeschlagen, nicht mitgeführt |

**Wogegen das Konzept nicht hilft:** Es schützt nicht gegen ein übernommenes
Gerät — wer es hat, hat den Nachweis. Es ersetzt keine Rechteprüfung in den
einzelnen Vorgängen; es sagt nur, **wer** ruft, nicht **ob er darf**. Und es
kennt keinen zweiten Faktor: Der Besitz eines eingerichteten Geräts ist der
einzige Nachweis.

## 6. Was das kostet

- **Ein weitergegebener Link wirkt, bis jemand handelt.** Ablauf und
  Zurückziehen wirken nur nach vorn; was der Link bereits erzeugt hat, muss
  einzeln entzogen werden. Das ist sichtbar, aber es ist Arbeit.
- **Der Ablauf erzwingt einen zweiten Vorgang.** Wer Einladungen verfallen lässt,
  braucht die Funktion „neue Einladung für einen vorhandenen Benutzer" (F2) —
  sonst entstehen Dubletten der Person. Das ist kein Zusatz, sondern der Preis
  des Ablaufs.
- **Kein Zugang ohne jemanden, der einlädt.** Auf einem frischen System ist der
  Bootstrap der einzige Weg hinein; geht das Bootstrap-Geheimnis verloren, hilft
  nur ein Zugriff auf die Ablage.
- **Nachweise sind langlebig — und damit ist ihr Verlust folgenreich.** Das
  Modell steht und fällt damit, dass ein verlorenes Gerät auch tatsächlich
  gesperrt wird; automatisch geschieht das nie.
- **Zwei Welten statt einer.** Menschen und Maschinen werden getrennt verwaltet.
  Das ist der Punkt, kostet aber zwei Oberflächen, zwei Listen, zwei Wege.
- **Rechte nachschlagen kostet einen Zugriff mehr**, bei jedem Aufruf.
- **Ohne Passwort gibt es nichts, das ein Mensch wiederholen kann.** Wer sein
  Gerät verliert und keinen Weg hat, sich selbst einen neuen Link zu holen, ist
  auf jemand anderen angewiesen.

## 7. Geltung und Grenzen

**Randbedingungen.** Ein Kreis, der **benannt** werden kann — jemand weiß, wer
dazugehört. Clients, die ein Cookie behalten dürfen. Eine Stelle, die einlädt.
Ein Deployment, das ein Bootstrap-Fenster öffnen kann.

**Kipp-Bedingungen.** Das Modell wird falsch, wenn:

- **jeder sich selbst anmelden können soll** — dann ist Zugehörigkeit keine
  Entscheidung mehr, und die Einladung ist eine Attrappe;
- der Kreis so groß wird, dass **niemand mehr überblickt**, wer eingeladen wurde
  — die Sichtbarkeit aus F3 ersetzt Verhinderung nur, solange jemand hinsieht;
- **regulatorisch ein zweiter Faktor** verlangt ist — der Besitz eines
  eingerichteten Geräts ist genau einer;
- **Nachweise kurzlebig sein müssen**, etwa auf geteilten Rechnern; dieses
  Modell lebt von langlebigen Nachweisen und wird bei kurzen Laufzeiten zur
  Zumutung.

**Nicht gelöst.** Dieses Dossier gibt **kein Rechtemodell** vor — welche
Fachrechte es gibt, wie fein sie geschnitten sind und wer sie bekommt,
entscheidet die Anwendung. Was es **nicht** offenlässt, sind die fünf
Anforderungen aus F10 an jedes Rechtesystem, das hier aufsetzt; insbesondere
bleibt die Zuständigkeit für Einladen und Entziehen nicht der Improvisation
überlassen. Es sagt nichts über die **Gestalt der Schnittstelle**,
über die die Nachweise reisen, und nichts über die **Auslieferung der Anwendung**
selbst. Und es beschreibt keinen Weg, ein verlorenes Gerät **automatisch** zu
erkennen; das bleibt eine menschliche Entscheidung.

## 8. Bewährung

> **Beispiel** (eine Inventarverwaltung; Namen und Werte erfunden), von der
> Einladung bis zum Maschinenzugang.
>
> **Einladen.** Ein Verwalter legt einen Benutzer „Anna" mit ihrer Adresse an
> und erzeugt eine Einladung. Heraus kommt ein Link:
>
> ```
> https://…/#einladung=k7Qx2mVd9pLw
> ```
>
> Alles hinter dem `#` sieht der Server nie (F9).
>
> **Einlösen.** Anna öffnet den Link auf dem Handy. Die Anwendung liest den
> Token aus dem Fragment und schickt ihn **einmal**; der Server legt ein Gerät
> an, fragt nach einer Bezeichnung — Anna tippt „Handy Anna" — und antwortet mit
> einem Cookie, das er selbst setzt. Ab jetzt ist der Einladungstoken für dieses
> Gerät bedeutungslos (F3).
>
> Am Abend öffnet sie denselben Link am Tablet. Es entsteht ein **zweites**
> Gerät mit eigenem Nachweis, „Tablet Küche" (F4). Beide Geräte halten fest, aus
> welcher Einladung sie stammen; die Verwaltung zeigt deshalb nicht „zwei
> Einlösungen", sondern **diese beiden Geräte** (F3).
>
> **Verlieren.** Das Tablet verschwindet. Der Verwalter entzieht **dieses eine**
> Gerät; Annas Handy funktioniert weiter, und im Verlauf ist ablesbar, was vom
> Tablet aus geändert wurde (F4). Beim nächsten Zugriff des Tablets endet die
> Prüfung in Schritt 2 — sofort, ohne dass ein Nachweis ablaufen müsste (F5).
>
> **Aufräumen.** Weil Annas Einladung nach der eingestellten Frist ohnehin
> verfällt, muss niemand daran denken, sie zurückzuziehen. Braucht Anna später
> ein drittes Gerät, erzeugt der Verwalter **eine neue Einladung für den
> vorhandenen Benutzer** — kein zweiter Eintrag „Anna" (F2).
>
> **Rechte.** Anna bekommt Verwalterrecht. Sie muss sich nirgends neu anmelden —
> beim nächsten Zugriff schlägt der Server das Recht am Benutzer nach und
> findet es, auf beiden Geräten (F5). Beim Entzug gilt dasselbe, sofort.
>
> **Maschine.** Ein Rechner soll nachts sichern. Er bekommt **kein** Konto,
> sondern einen Fähigkeitsschlüssel — und zwar so:
>
> Das Auslieferungsskript legt bei seinem Lauf neben der Konfiguration eine Datei
> an, etwa `setup-config.php`, mit zwei Werten:
>
> ```php
> <?php return [
>   'setup_token' => 'A7fK…',            // dasselbe Geheimnis, das der Einrichtende hat
>   'gueltig_bis' => '2026-08-15T19:42', // eine Stunde nach dem Lauf
> ];
> ```
>
> Solange die Uhr darunter liegt, sind die Setup-Vorgänge offen. Der Einrichtende
> lässt die API in diesem Fenster **alle** nötigen Maschinenschlüssel erzeugen —
> einen mit dem Zweck *sichern* für den Sicherungsrechner, einen mit dem Zweck
> *auswerten* für den Auswertungsrechner, jeder benannt nach seiner Maschine (F6).
> Jeder Klartext erscheint **genau einmal** und wandert in die Konfiguration des
> jeweiligen Rechners. Danach läuft die Stunde ab, und dieselben Aufrufe
> antworten, dass das Fenster zu ist (F7, F8).
>
> **Wer diese Datei schreiben konnte, hatte ohnehin Zugriff auf den Rechner** —
> das Fenster gibt ihm nichts, was er nicht schon hatte.
>
> Kommt ein halbes Jahr später ein dritter Rechner dazu, ist das kein Sonderfall:
> ausliefern, im dann wieder offenen Fenster seinen Schlüssel erzeugen, fertig.
>
> **Widerrufen geht dagegen jederzeit.** Wird der Auswertungsrechner
> kompromittiert, legt ein Verwalter dessen Schlüssel **sofort** still — ohne
> Deployment, ohne Fenster (F7). Der Sicherungsschlüssel bleibt davon unberührt;
> und weil er den Zweck *sichern* trägt, war mit dem Auswertungsrechner ohnehin
> nie der ganze Bestand erreichbar.
>
> **Der Fehler, den dieses Modell verhindert:** In der Benutzerliste steht kein
> Eintrag „Backup-Dienst".

**Referenz-Implementierung: entfällt, weil noch keine existiert (Status:
Entwurf).** Das Modell ist vollständig entschieden und spezifiziert; die
laufende Anwendung, aus der es stammt, bildet es noch nicht ab — sie stammt aus
einer Zeit, in der es eine anonyme Lesestufe gab, also aus dem Zustand, den F1
ausschließt. Die Aussagen dieses Dossiers sind damit **durchdacht, nicht
erprobt**; insbesondere die Behauptung aus F3, dass Sichtbarkeit statt
Verhinderung im Alltag genügt, ist bisher ein Argument.

## 9. Offene Fragen

**Q1 · Wie lang ist die richtige Frist?** F3 empfiehlt einen Ablauf, nennt aber
keine Zahl. Zu kurz, und der Einladende schickt ständig neue Links; zu lang, und
der Ablauf ist eine Beruhigung ohne Wirkung. Vermutlich hängt es daran, wie
schnell Eingeladene tatsächlich reagieren — dazu fehlen Erfahrungswerte.

**Q2 · Was passiert, wenn das Bootstrap-Geheimnis verloren geht?** F8 macht es
zum stärksten Geheimnis des Systems und legt es außerhalb des Bestands ab. Geht
es verloren, ist der Weg zurück ein Zugriff auf die Ablage. Ist das die
akzeptierte Antwort, oder braucht es einen zweiten, ebenso geschützten Weg?

**Q3 · Wie erkennt man ein Gerät wieder, ohne es zu vermessen?** F4 stützt sich
auf eine vom Menschen vergebene Bezeichnung, weil technische Merkmale
unzuverlässig sind. Bei zehn Geräten derselben Person hilft das wenig. Gibt es
eine Wiedererkennung, die nicht auf einem Fingerabdruck des Geräts beruht?

**Q4 · Sollte ein Fähigkeitsschlüssel ablaufen?** F7 macht ihn zu einem
bewussten Akt, aber nicht zu einem befristeten. Für Fähigkeitsschlüssel im
Allgemeinen ist die Frage anderswo bejaht — Verfall als Pflicht des Betreibers
(Abschnitt 10). Hier stellt sie sich schärfer, weil die Erzeugung an ein
Deployment gebunden ist (F7): Ein Ablauf zwänge zu einem Deployment zu einem
Zeitpunkt, den die Frist bestimmt und nicht der Betrieb. Ist das ein Gewinn oder
eine neue Fehlerquelle — und wäre die Antwort eine Frist, die beim Deployment
verlängert wird, statt einer, die von selbst zuschlägt?

## 10. Quellen & Verwandtes

- **HTTP State Management (Cookies)**, RFC 6265 und der Nachfolgeentwurf — die
  Festlegungen zu `HttpOnly`, `Secure`, `SameSite` und zur wirksamen
  Höchstlaufzeit von 400 Tagen, an der die Langlebigkeit der Nachweise ihre
  Grenze findet: <https://www.rfc-editor.org/rfc/rfc6265.html> (abgerufen
  2026-08-15).
- **Fragment-Bezeichner in Adressen**, RFC 3986 Abschnitt 3.5 — die Festlegung,
  dass der Fragment-Teil nicht an den Server übertragen wird, auf der F9 beruht:
  <https://www.rfc-editor.org/rfc/rfc3986#section-3.5> (abgerufen 2026-08-15).
- **OWASP Session Management Cheat Sheet**, zur Einordnung der hier getroffenen
  Abweichungen von der üblichen Anmeldelösung:
  <https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html>
  (abgerufen 2026-08-15).
- **Vertiefung, nicht Voraussetzung — die Fähigkeitsseite im Einzelnen:** Das
  Schwester-Dossier `capability-tokens-for-agents`
  (<https://github.com/schlingensiepen/proposals/tree/main/capability-tokens-for-agents>)
  behandelt ausschließlich die zweite der beiden Welten aus F6, und zwar
  wesentlich tiefer: Zuschnitt (ein Token, eine Fähigkeit, ein Ort), Übergabe
  über einen zweiten Kanal, Ablage in fester Rangfolge, Rotation, Verfall und
  die Regeln, an die sich ein maschineller Verwender halten muss.

  **Die Grenze zwischen beiden verläuft so:** Dort geht es um die
  Fähigkeitsseite allein, mit einem **Agenten** als Verwender, der sich selbst
  anmeldet und sein Token über einen zweiten Kanal erhält. Hier geht es um die
  **Grenze** zwischen Identität und Fähigkeit in einem System, das beides führt —
  und der maschinelle Verwender ist ein Prozess, den der Betreiber ausliefert;
  sein Schlüssel entsteht deshalb im Deployment (F7) und nicht durch
  Selbstanmeldung. Das ist kein Widerspruch, sondern ein anderer Ausgangsfall:
  Ein Agent taucht auf, ein Hintergrundprozess wird hingestellt.

- **Vertiefung, nicht Voraussetzung:** Wie die Nachweise transportiert werden —
  ausschließlich in Kopfzeilen, nie im Rumpf und nie in der Adresse — und wie ein
  Fehlerobjekt aussieht, das „nicht angemeldet" von „nicht berechtigt"
  unterscheidet, beschreibt das Schwester-Dossier `single-endpoint-json-rpc`
  (<https://github.com/schlingensiepen/proposals/tree/main/single-endpoint-json-rpc>).
  Dieses Dossier ist ohne jenes vollständig: Es legt fest, **welche** Nachweise
  es gibt und **wie sie entstehen**, nicht, über welche Schnittstelle sie reisen.
