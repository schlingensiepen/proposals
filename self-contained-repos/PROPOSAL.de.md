---
type: Konzept
status: Entwurf
version: "0.9 · Materialfassung zur Fertigstellung"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: self-contained-repos
language: de (führend)
question: "Wie schneide ich ein Repository so zu, dass ein Mensch oder ein Agent, der es zum ersten Mal öffnet, allein daraus arbeiten kann — ohne Vorwissen, ohne Nachfragen, ohne fremde Dokumente?"
thesis: "Ein Repository trägt alles, was zum Verstehen und Mitarbeiten nötig ist, in sich: einen Einstieg, der auf die zwei oder drei tatsächlichen Aufgaben verzweigt, Entscheidungen samt ihrer Begründung im Volltext statt als Verweis, und eine Prüfregel dafür — funktioniert es noch, wenn jeder externe Link tot ist?"
---

<!-- MATERIALFASSUNG. Enthält vollständig, was in dieser Arbeit zum Thema entschieden und
     erwogen wurde, einschließlich der verworfenen Wege. Noch nicht durch die Endredaktion
     der Reihe: englische Fassung, AGENT.md, README und CHANGELOG fehlen; §8 ist zu
     ergänzen, sobald das Verfahren länger im Betrieb ist. Konventionen in AGENTS.md und
     agents/create-or-maintain.md des Repositoriums. F- und Q-Nummern ab jetzt stabil. -->

# Das selbsttragende Repository

**In einem Satz:** Ein Repository ist selbsttragend, wenn jemand, der es zum ersten Mal öffnet — Mensch oder Agent —, ohne Rückfrage und ohne fremde Dokumente arbeiten kann: Ein Einstieg verzweigt auf die tatsächlichen Aufgaben, jede Entscheidung steht mit ihrer Begründung im Volltext darin, und die Prüfregel lautet, ob alles noch trägt, wenn jeder externe Link tot ist.

**Kurzthese:** Wissen über ein Vorhaben verteilt sich naturgemäß: ins Ticketsystem, ins Wiki, in Chatverläufe, in die Köpfe. Wer dann das Repository öffnet, findet Code und eine Bedienungsanleitung — aber nicht, warum etwas so ist, und nicht, was von ihm erwartet wird. Für einen Menschen ist das lästig; für einen Agenten ist es das Ende der Arbeitsfähigkeit, denn er kann nicht im Flur nachfragen. Selbsttragend heißt: Alles, was zum Verstehen und Mitarbeiten nötig ist, liegt im Repository, und was von außen kommt, wird mit Aussage und Begründung hineingeholt statt verlinkt.

## 1. Problem

Der Autor arbeitet mit KI-Agenten an mehreren Repositorien und hat dabei eine Beobachtung gemacht, die für Menschen genauso gilt, nur weniger schmerzhaft: **Ein Agent kann nicht nachfragen.** Er hat keinen Flur, keinen Kollegen und kein Gedächtnis aus dem letzten Projekt. Was nicht im Repository steht, existiert für ihn nicht — und was er nicht findet, erfindet er, in bester Absicht und mit voller Überzeugung.

Der übliche Zustand eines Repositoriums ist das Gegenteil davon. Der Code liegt darin, eine Bedienungsanleitung meistens auch. Aber das *Warum* liegt woanders: in Entscheidungsprotokollen eines Ticketsystems, in einer Wiki-Seite, in einem Entwurfsdokument auf einem Laufwerk, in einem Chatverlauf, den niemand mehr findet. Und die Erwartung an den, der mitarbeitet, liegt gar nirgends — sie ist Erfahrungswissen. Wer neu dazukommt, stellt Fragen; wer keine Fragen stellen kann, rät.

Das hat drei sichtbare Folgen. **Verworfene Wege werden wieder vorgeschlagen**, weil niemand mehr weiß, dass sie schon einmal geprüft und aus einem guten Grund verworfen wurden — und der Grund fehlt genau dann, wenn jemand die Entscheidung anzweifelt. **Verweise verrotten**: Ein Link auf ein internes Dokument ist heute eine Abkürzung und in zwei Jahren eine Sackgasse; das Wissen dahinter ist dann verloren, obwohl das Repository noch vollständig aussieht. Und **jeder Neuzugang kostet dieselbe Einarbeitung**, weil sie nirgends niedergeschrieben ist, sondern jedes Mal mündlich stattfindet.

Drei Rollen tragen das Problem: **Der Verfasser**, der beim Schreiben nicht merkt, was er stillschweigend voraussetzt. **Der Übernehmende**, der Monate später Entscheidungen vorfindet, deren Begründung niemand mehr kennt. Und **der Agent**, der ohne Kontext in dieselbe Lage gerät wie ein Neuzugang am ersten Tag — nur dass er nicht fragen, sondern nur raten kann.

## 2. Das Konzept

Die Antwort ist eine Verschiebung: **Wissen wird hineingeholt, nicht verlinkt.** Ein selbsttragendes Repository enthält nicht nur, was das System tut, sondern auch, warum es so ist und was von dem erwartet wird, der daran arbeitet. Drei Bestandteile tragen das.

Erstens ein **Einstieg für Mitarbeitende**, der nicht erklärt, sondern verzweigt. Ein Repository hat in der Praxis zwei oder drei wirklich verschiedene Aufgaben — etwas ändern, etwas verwenden, etwas nachschlagen —, und der Einstieg benennt sie und schickt den Ankommenden zu genau der Anweisung, die zu seiner Aufgabe gehört. Er ist ein Wegweiser, kein Handbuch: kurz genug, dass ihn jeder liest, bevor er anfängt.

Zweitens die **Inlining-Regel für Entscheidungen**: Wenn ein Dokument im Repository auf eine anderswo getroffene Entscheidung Bezug nimmt, werden **Aussage und Begründung** in das Dokument übernommen. Ein Verweis auf das Entscheidungsprotokoll ist allenfalls ein Wartungsvermerk, nie eine Verständnisvoraussetzung. Redundanz wird dabei bewusst in Kauf genommen — sie ist billiger als die Kopplung.

Drittens die **Prüfregel**, die aus dem Vorsatz eine überprüfbare Aussage macht: *Versteht und befolgt man das Repository noch, wenn jeder externe Link tot ist?* Diese Frage lässt sich beim Gegenlesen tatsächlich stellen, und sie schlägt bei jeder verlinkten Verständnisvoraussetzung an.

Dazu kommt eine Erkenntnis, die erst in der Praxis entstand: **Wer eine Anweisung für Agenten schreibt, darf sie nicht an Nummern aus einem anderen Dokument hängen.** Eine Prüfliste, die auf „siehe Festlegung 3" verweist, zwingt den Agenten doch wieder zum Volltext — dann wäre die eigene Datei sinnlos. Verweise auf das Herkunftsdokument dürfen deshalb nur als Herkunftsvermerk in Klammern stehen; die Regel selbst muss ausgeschrieben sein.

## 3. Begriffe

**Selbsttragend** — verständlich und befolgbar aus sich heraus, ohne Zugriff auf Dokumente außerhalb des Repositoriums.

**Einstieg** — die eine Datei, die ein Ankommender zuerst liest und die ihn auf die zu seiner Aufgabe passende Anweisung verzweigt.

**Aufgabendefinition** — eine Anweisung für genau eine wiederkehrende Tätigkeit an diesem Repository, aus sich heraus befolgbar.

**Entscheidung** — eine getroffene Festlegung mit ihrer Begründung und den erwogenen Alternativen; oft in einem eigenen Protokoll geführt.

**Inlining** — das Übernehmen von Aussage und Begründung einer Entscheidung in das Dokument, das sie braucht, statt eines Verweises.

**Herkunftsvermerk** — ein Verweis auf die Quelle einer Aussage, der ausschließlich der Wartung dient und für das Verständnis nicht gebraucht wird.

**Verständnisvoraussetzung** — ein Verweis, ohne dessen Ziel man den Text nicht befolgen kann; genau das, was in einem selbsttragenden Repository nicht vorkommen darf.

**Verweis-Verrottung** — der Zustand, in dem ein Link ins Leere zeigt und das Wissen dahinter verloren ist, obwohl das verweisende Dokument noch vollständig wirkt.

## 4. Festlegungen

**Landkarte.** Sieben Festlegungen. **F1** ist die Grundregel und die Prüffrage. **F2–F4** betreffen die Struktur: Einstieg, Aufgabendefinitionen, Selbsttragung der Anweisungen. **F5** ist die Inlining-Regel für Entscheidungen. **F6** und **F7** betreffen die Pflege: was bei Änderungen mitzuziehen ist und wie mit erzeugten Dateien umgegangen wird. F1 trägt alles; F5 ist die Festlegung, an der es in der Praxis am häufigsten scheitert.

### F1 · Die Prüfregel: Funktioniert es mit toten Links?

**Gilt für:** alle, die im Repository schreiben.

**Was gilt.** Jedes Dokument im Repository muss verständlich und befolgbar bleiben, wenn jeder Verweis nach außen ins Leere zeigt. Externes Wissen wird mit Aussage und Begründung hereingeholt; Verweise sind Belege oder Vertiefung, nie Voraussetzung. Redundanz ist dafür ausdrücklich zulässig.

**Warum.** Die Regel macht aus einem Vorsatz eine Prüffrage, die beim Gegenlesen tatsächlich gestellt werden kann — und die zuverlässig anschlägt. Sie trifft außerdem genau den Unterschied zwischen Mensch und Agent: Der Mensch klickt einem toten Link hinterher und fragt notfalls nach; der Agent tut weder das eine noch das andere, sondern füllt die Lücke mit Erfindung. Und sie schützt vor einer Täuschung, die sonst unbemerkt bleibt: Ein Dokument mit toten Links sieht vollständig aus.

**Naheliegend und verworfen.** *Auf verlässliche Links vertrauen* — Links auf interne Systeme überleben Umzüge, Umbenennungen und Werkzeugwechsel nicht; die Halbwertszeit ist kürzer als die des Repositoriums. *Redundanz vermeiden („eine Quelle der Wahrheit")* — richtig für Daten, falsch für Erklärungen: Eine Erklärung, die nur an einer Stelle steht, ist an allen anderen Stellen abwesend. Der Preis ist, dass Redundanz gepflegt werden muss (F6). *Ein Verweis-Prüfer im Bau* — findet tote Links, aber nicht das eigentliche Problem: dass ein lebendiger Link eine Verständnisvoraussetzung ist.

**Woran man merkt, dass es verletzt ist.** Ein Leser muss einem Link folgen, um weiterarbeiten zu können — oder jemand beantwortet eine Frage zum Repository aus dem Gedächtnis statt aus dem Repository.

### F2 · Ein Einstieg, der verzweigt statt erklärt

**Gilt für:** Verfasser.

**Was gilt.** Es gibt genau eine Datei, die ein Ankommender zuerst liest. Sie erklärt das Vorhaben nicht, sondern benennt die zwei oder drei tatsächlichen Aufgaben, die an diesem Repository anfallen, und verweist je Aufgabe auf die zugehörige Anweisung. Sie ist kurz genug, dass sie vor der Arbeit gelesen wird, und sie steht an dem Ort, an dem die verbreiteten Agenten-Werkzeuge zuerst nachsehen; wo mehrere solcher Orte üblich sind, zeigen die weiteren nur auf diesen einen.

**Warum.** Ein Ankommender kommt mit einer Absicht, nicht mit Lesehunger — genau wie ein Agent mit einem Auftrag. Ein Einstieg, der erklärt, wird überflogen; einer, der verzweigt, wird benutzt. Die Beschränkung auf zwei oder drei Aufgaben ist dabei kein Formalismus: Wer mehr aufzählt, hat meist keine Aufgaben mehr benannt, sondern Themen — und der Ankommende weiß wieder nicht, wo er hingehört.

**Naheliegend und verworfen.** *Die Bedienungsanleitung als Einstieg* — sie richtet sich an Benutzer des Erzeugnisses, nicht an Mitarbeitende am Repository; beide Absichten in einem Dokument bedienen heißt, keine davon gut zu bedienen. *Mehrere gleichwertige Einstiegsdateien für verschiedene Werkzeuge* — driften auseinander; einer führt, die anderen zeigen auf ihn. *Kein Einstieg, dafür sprechende Verzeichnisnamen* — funktioniert für den, der die Konvention schon kennt, also für niemanden, der neu ist.

**Woran man merkt, dass es verletzt ist.** Ein Neuzugang fragt „wo fange ich an?" — oder zwei Einstiegsdateien sagen Verschiedenes.

### F3 · Eine Aufgabendefinition je wiederkehrender Tätigkeit

**Gilt für:** Verfasser.

**Was gilt.** Für jede Tätigkeit, die am Repository wiederkehrend anfällt, gibt es eine eigene Anweisung: was zu tun ist, in welcher Reihenfolge, woran man erkennt, dass es fertig ist. Sie beschreibt die Tätigkeit vollständig — nicht das Vorhaben, nicht das Werkzeug, nicht die Motivation.

**Warum.** Wiederkehrende Tätigkeiten sind der Ort, an dem Erfahrungswissen sonst hängen bleibt: Jeder macht es ein bisschen anders, und wer neu ist, macht es falsch, ohne dass jemand es merkt. Aufgeschrieben werden sie zu etwas, das ein Agent unverändert ausführen kann — und zu einer Prüfliste, gegen die ein Mensch sich selbst kontrolliert.

**Naheliegend und verworfen.** *Alles in eine große Mitarbeitenden-Anleitung* — wird lang, und der Ausführende muss seinen Teil erst heraussuchen; genau das erledigt der Einstieg (F2) besser. *Die Tätigkeit im Ticket beschreiben* — gilt für dieses eine Mal und ist beim nächsten Mal weg. *Auf allgemeine Konventionen verweisen* — die kennt jeder anders.

**Woran man merkt, dass es verletzt ist.** Dieselbe Tätigkeit wird von zwei Personen verschieden ausgeführt, und beide halten sich für richtig.

### F4 · Anweisungen sind selbsttragend, auch gegenüber dem eigenen Repository

**Gilt für:** Verfasser.

**Was gilt.** Eine Anweisung, die einzeln weitergegeben werden kann, ist auch gegenüber den übrigen Dateien des Repositoriums selbsttragend: Jede Regel und jede Prüffrage steht darin ausgeschrieben. Verweise auf Nummern oder Abschnitte anderer Dokumente stehen nur als Herkunftsvermerk in Klammern und dürfen zum Befolgen nicht nötig sein.

**Warum.** Diese Festlegung entstand aus einem konkreten Einwand: Eine Prüfliste, die ihre Punkte an Nummern eines anderen Dokuments hängt, zwingt den Ausführenden doch wieder, das andere Dokument vollständig zu laden — dann ist die eigene Datei sinnlos, und der ganze Aufwand ihrer Trennung war umsonst. Der Herkunftsvermerk bleibt trotzdem nützlich, aber für einen anderen Zweck: Er dient dem Pflegenden beim Abgleich, nicht dem Ausführenden beim Arbeiten.

**Naheliegend und verworfen.** *Kurze Prüfliste mit Verweisen, Langfassung im Hauptdokument* — spart Platz und kostet Brauchbarkeit. *Die Anweisung ganz weglassen und nur das Hauptdokument pflegen* — zwingt jeden Ausführenden, ein Diskussionspapier zu lesen, um eine Handlungsanweisung zu bekommen. *Automatisch aus dem Hauptdokument erzeugen* — wurde erwogen; die Anweisung hat aber eine andere Form als die Begründung (Befehl statt Argument), und eine mechanische Ableitung erzeugt entweder Befehle, die argumentieren, oder Argumente, die befehlen.

**Woran man merkt, dass es verletzt ist.** Wer die Anweisung befolgen will, muss ein zweites Dokument öffnen.

### F5 · Entscheidungen kommen mit Begründung ins Dokument

**Gilt für:** Verfasser.

**Was gilt.** Wo ein Dokument sich auf eine Entscheidung stützt, werden **Aussage und Begründung** übernommen — und, wo es die Sache trägt, auch die erwogenen und verworfenen Alternativen mit dem jeweiligen Grund der Verwerfung. Ein Verweis auf das Entscheidungsprotokoll ist ein Wartungsvermerk, nie eine Verständnisvoraussetzung.

**Warum.** Die Begründung ist der teuerste Teil einer Entscheidung und der erste, der verloren geht. Ohne sie wird die Entscheidung beim ersten Widerstand aufgegeben, weil niemand mehr weiß, was sie verhindert. Und die verworfenen Alternativen sind noch teurer, wenn sie fehlen: Wer sie nicht liest, schlägt die verworfene Lösung wieder vor — freundlich, plausibel und ohne dass jemand den Grund erinnert. Ein Entscheidungsprotokoll bleibt trotzdem sinnvoll: Es ist die Historie mit Datum und Verlauf. Nur ist es nicht der Ort, an dem gearbeitet wird.

**Naheliegend und verworfen.** *Nur verweisen („siehe Entscheidung 12")* — spart Schreibarbeit; genau das ist die Kopplung, die F1 verbietet. *Nur die Aussage übernehmen, die Begründung weglassen* — erzeugt Regeln, die wie Willkür aussehen und entsprechend behandelt werden. *Das Protokoll ins Repository legen und damit gut sein* — es ist chronologisch geordnet, das Arbeitsdokument sachlich; wer im Protokoll sucht, sucht lange.

**Woran man merkt, dass es verletzt ist.** Eine verworfene Lösung wird erneut vorgeschlagen — oder jemand fragt „warum eigentlich?" und niemand kann es beantworten, ohne ein anderes Dokument zu holen.

### F6 · Redundanz wird gepflegt, nicht vermieden

**Gilt für:** Verfasser · Pflegende.

**Was gilt.** Weil Wissen bewusst mehrfach im Repository steht, gehört zu jeder inhaltlichen Änderung die Frage, welche anderen Stellen sie berührt — und die Antwort gehört in denselben Änderungsvorgang. Für die wichtigsten Paarungen (Diskussionspapier und Anweisung, Anweisung und Prüfliste, Dokument und Registereintrag) wird das ausdrücklich festgehalten, statt es dem Gedächtnis zu überlassen.

**Warum.** Redundanz ist die Gegenleistung für Selbsttragung, und sie hat einen Preis: Zwei Stellen können auseinanderlaufen. Der Unterschied zwischen tragfähiger und fauler Redundanz ist genau diese Pflege. Wer sie nicht organisiert, bekommt das Schlechteste aus beiden Welten — Kopien, die auseinanderdriften, ohne dass jemand es merkt, weil beide für sich plausibel aussehen.

**Naheliegend und verworfen.** *Werkzeuge, die Kopien synchron halten* — funktionieren für Textbausteine, nicht für Wissen, das in verschiedenen Formen erscheinen muss (Befehl versus Begründung). *Redundanz doch vermeiden und wieder verlinken* — dann fällt F1. *Auf Sorgfalt vertrauen* — Sorgfalt ohne benannte Paarungen ist Zufall.

**Woran man merkt, dass es verletzt ist.** Zwei Stellen im Repository sagen dasselbe verschieden — und niemand weiß, welche recht hat.

### F7 · Erzeugtes ist als erzeugt gekennzeichnet, mit benannter Quelle

**Gilt für:** Verfasser · Pflegende.

**Was gilt.** Wo Dateien aus anderen Dateien erzeugt werden — aufgeteilte Entscheidungsdokumente, Verzeichnisse, Übersichten —, tragen sie einen sichtbaren Hinweis, dass sie erzeugt sind, und benennen die führende Quelle. Änderungen geschehen ausschließlich an der Quelle; die Erzeugung läuft automatisch mit, damit Abweichungen auffallen.

**Warum.** Erzeugte Dateien sehen aus wie geschriebene, und genau darin liegt die Falle: Jemand ändert die Kopie, die Änderung verschwindet beim nächsten Lauf, und niemand versteht, warum. Der Hinweis kostet eine Zeile und beantwortet die Frage, bevor sie entsteht. Wichtig ist, dass diese Festlegung F1 nicht verletzt: Die erzeugte Datei ist vollständig lesbar; der Hinweis auf die Quelle ist ein Wartungsvermerk, keine Verständnisvoraussetzung.

**Naheliegend und verworfen.** *Erzeugtes nicht einchecken* — dann fehlt es dem, der nur das Repository hat; für ein selbsttragendes Repository ist gerade die Verfügbarkeit ohne Bauwerkzeuge der Punkt. *Erzeugtes von Hand pflegen* — driftet sofort. *Nur in der Bau-Konfiguration dokumentieren, dass erzeugt wird* — findet niemand, der die Datei vor sich hat.

**Woran man merkt, dass es verletzt ist.** Jemand ändert eine erzeugte Datei, und die Änderung verschwindet.

**Tragwerk (Kurzrekapitulation).** F1 trägt alles: Ohne die Prüfregel sind F4 und F5 nur guter Wille. F2 und F3 sind die Struktur, in der Selbsttragung überhaupt eine Form bekommt — ohne Einstieg findet niemand die Anweisung, ohne Anweisungen gibt es nichts zu finden. F5 ist die Festlegung mit dem größten Ertrag und dem meisten Widerstand, weil sie Schreibarbeit verlangt, deren Nutzen erst später sichtbar wird. F6 ist der Preis für F1 und F4 zusammen; wird er nicht bezahlt, kippt die Selbsttragung in Widersprüchlichkeit. F7 ist unabhängig und gilt auch für Repositorien, die von diesem Konzept sonst nichts übernehmen.

## 5. Sicherheitsbetrachtung

| Risiko | Typischer Hergang | Gegenmaßnahmen |
|---|---|---|
| Vertrauliches wandert beim Inlining mit | Beim Übernehmen einer Begründung werden interne Namen, Kundendaten oder Zugangswege mitkopiert | Beim Inlining wird die *Sache* übernommen, nicht der Kontext; vor der Veröffentlichung eines Repositoriums ist jedes hereingeholte Wissen auf Vertrauliches durchzusehen |
| Fremdes Wissen ohne Recht übernommen | Ein Abschnitt aus fremder Dokumentation wird hereinkopiert | Inlining heißt Aussage und Begründung in eigenen Worten, nicht Übernahme fremden Wortlauts; Belege gehören in den Quellen-Abschnitt |
| Anweisung wird zur Angriffsfläche | Eine Datei, die ein Agent unverändert befolgt, wird von jemandem verändert, der das Repository beschreiben darf | Änderungen an Anweisungsdateien sind wie Codeänderungen zu behandeln — dieselbe Durchsicht, dieselbe Historie; wer schreiben darf, weist einem Agenten Handlungen an |
| Falsche Sicherheit durch Vollständigkeitsschein | Ein Dokument wirkt vollständig, obwohl seine Verständnisvoraussetzungen tot sind | Genau dagegen ist F1 gerichtet; die Prüffrage gehört in die Durchsicht, nicht nur in die Absicht |

Grenze des Modells: Selbsttragung sagt nichts über Richtigkeit. Ein Repository kann vollständig aus sich heraus verständlich sein und trotzdem falsche Entscheidungen dokumentieren — dieses Konzept sorgt nur dafür, dass die Entscheidungen samt Begründung sichtbar sind und damit angreifbar werden.

## 6. Was das kostet

**Schreibarbeit beim Verfassen (F5).** Aussage, Begründung und verworfene Alternativen auszuschreiben dauert deutlich länger, als einen Verweis zu setzen — und der Nutzen tritt erst ein, wenn jemand anderes das Dokument liest oder die Entscheidung angezweifelt wird. Das ist die unbeliebteste Art von Arbeit: teuer jetzt, wertvoll später.

**Doppelte Pflege (F6).** Jede inhaltliche Änderung berührt mehrere Stellen. Wer das nicht organisiert, bekommt Widersprüche; wer es organisiert, bezahlt bei jeder Änderung.

**Längere Dokumente (F1, F5).** Selbsttragung erzeugt Wiederholung, und Wiederholung erzeugt Umfang. Wer Kürze für einen Wert an sich hält, wird diese Dokumente unangenehm finden.

**Die Struktur will gepflegt werden (F2, F3).** Aufgaben ändern sich; ein Einstieg, der auf drei Anweisungen verzweigt, von denen zwei veraltet sind, ist schlimmer als keiner.

**Erzeugung braucht eine Kette (F7).** Wer erzeugte Dateien eincheckt, braucht einen Lauf, der Abweichungen sichtbar macht — sonst ist der Quellenhinweis eine Behauptung.

**Der Ertrag ist unsichtbar.** Ein selbsttragendes Repository fällt niemandem auf; es fehlen nur die Rückfragen, die Fehlgriffe und die Wiederauferstehung verworfener Ideen. Das macht es schwer zu verteidigen, wenn jemand die Kosten sieht und den Nutzen nicht.

## 7. Geltung und Grenzen

### Randbedingungen

**Das Repository ist die Arbeitsgrundlage.** Wo tatsächlich in einem Wiki oder Ticketsystem gearbeitet wird und das Repository nur Code enthält, greift das Konzept ins Leere: Man kann Wissen nicht dorthin holen, wo niemand liest.

**Es gibt wiederkehrende Tätigkeiten.** Ein Repository ohne Mitarbeit — ein reines Archiv — braucht Einstieg und Aufgabendefinitionen nicht.

**Jemand pflegt.** Selbsttragung ohne Pflege (F6) hält ungefähr so lange wie die erste Änderung.

### Wann die Antwort kippt

**Regulatorische Einzigkeit ist vorgeschrieben.** Wo ein Qualitätssystem verlangt, dass eine Aussage genau einmal existiert und dort gelenkt wird, verbietet sich Inlining — dann bleibt nur der Verweis, und die Selbsttragung endet an dieser Stelle. Ehrlicher ist dann, sie ausdrücklich als nicht erreichbar zu benennen, statt sie zu behaupten.

**Das Wissen gehört jemand anderem.** Fremde Dokumentation kann man nicht hereinholen, nur zusammenfassen und belegen. Wo die Zusammenfassung nicht genügt, bleibt eine echte Voraussetzung bestehen.

**Das Repository ist zu groß für einen Einstieg.** Bei sehr vielen Teilbereichen wird der Einstieg entweder unvollständig oder zum Verzeichnis — dann braucht es Selbsttragung je Teilbereich, und die Frage nach der Klammer stellt sich neu.

**Der Inhalt ändert sich schneller, als Redundanz gepflegt werden kann.** Dann sind Widersprüche wahrscheinlicher als tote Links, und die Rechnung dreht sich um.

### Was es nicht löst

**Richtigkeit.** Siehe §5: Vollständig verständlich heißt nicht richtig.

**Auffindbarkeit über Repositorien hinweg.** Wer nicht weiß, dass es dieses Repository gibt, dem hilft sein Einstieg nicht.

**Berechtigungen und Freigabe.** Wer im Repository schreiben darf, ist eine andere Frage — sie wird durch F1 nur wichtiger, weil Anweisungsdateien Wirkung entfalten.

**Die Form der Dokumente.** Dieses Konzept sagt, was enthalten sein muss, nicht, in welchem Format oder welcher Gliederung.

## 8. Bewährung

**In Betrieb: die Dossier-Reihe des Autors.** Das Verfahren wird in dem Repository angewendet, in dem dieses Dossier selbst liegt. Umgesetzt sind: ein Einstieg, der auf zwei Aufgaben verzweigt — ein Dossier erstellen oder pflegen, ein Dossier finden und verwenden — und je Aufgabe eine eigene Anweisung; eine ausdrückliche Verweis-Regel, nach der Verweise ausschließlich im Quellen-Abschnitt stehen; die Inlining-Regel für Entscheidungen; die Prüfregel „funktioniert es mit toten Links?" als Abschnitt des Wurzel-Einstiegs; selbsttragende Agenten-Anweisungen je Dossier, in denen Nummern des Diskussionspapiers nur als Herkunftsvermerk erscheinen; und ein Wegweiser für ein zweites verbreitetes Agenten-Werkzeug, der ausschließlich auf den einen Einstieg zeigt.

**Ein Beleg aus der Praxis, ungeplant.** Bei einem Prüflauf sollte ein kontextfreier Agent allein aus einem Dossier ein Werkzeug bauen. Er meldete unter anderem, das Dossier verweise für eine Regel auf ein Schwester-Dokument, das ihm nicht vorlag — es sei „streng genommen nicht vollständig in sich geschlossen". Genau die Prüfregel aus F1, angewendet von jemandem, der sie nicht kannte. Die Stelle wurde ausgeschrieben; ein zweiter Prüflauf mit einem frischen Agenten bestätigte ausdrücklich, dass keine Verständnisvoraussetzung mehr nach außen zeigt. *Dies ist derzeit der belastbarste Beleg des Konzepts und sollte bei der Fertigstellung ausgeführt werden.*

**Herkunft der Inlining-Regel.** Sie entstand als ausdrückliche Entscheidung des Autors gegenüber einer Schablone, die für Entscheidungen nur Herkunftsverweise vorsah. Die Begründung damals: Ein Dokument, das für seine Regeln auf ein Entscheidungsprotokoll zeigt, ist ohne dieses Protokoll nicht befolgbar — also gehört beides hinein. Ebenso entstand F4 aus einem Einwand gegen eine Prüfliste, die ihre Punkte an Nummern des Hauptdokuments hängte: „Dann ist die einzelne Datei sinnlos."

*Zur Fertigstellung: Betriebserfahrung über längere Zeit fehlt; insbesondere ist offen, wie gut sich F6 in der Praxis durchhalten lässt. Der Abschnitt sollte ehrlich vermerken, dass die Reihe zum Zeitpunkt der Niederschrift wenige Wochen alt war.*

## 9. Offene Fragen

**Q1** *(Auftrag an den Verwender)* Welche Paarungen im eigenen Repository müssen bei einer Änderung zusammen angefasst werden (F6)? Die Liste ist repositoriumsabhängig; das Konzept verlangt nur, dass es sie gibt und dass sie aufgeschrieben ist.

**Q2** Lässt sich die Prüfregel aus F1 maschinell unterstützen? Tote Links findet ein Werkzeug; die eigentliche Frage — ist dieser lebendige Link eine Voraussetzung? — vermutlich nicht. Wäre eine Konvention hilfreich, die Verweise als „Beleg" oder „Voraussetzung" auszeichnet, sodass Letztere überhaupt auffallen?

**Q3** Wo verläuft die Grenze des Inlinings bei fremden Normen und Standards? Deren Aussage lässt sich zusammenfassen, ihr Wortlaut nicht übernehmen. Braucht es dafür eine eigene Regel — etwa: die *Wirkung* hereinholen, die *Fundstelle* belegen?

**Q4** Wie hält man Selbsttragung in einem Repository durch, das über die Zeit sehr groß wird (§7)? Gilt sie dann je Teilbereich, und was hält die Teilbereiche zusammen?

**Q5** Verhält sich dieses Konzept zu den verbreiteten Konventionen für Agenten-Einstiegsdateien so, dass es sie ergänzt oder ersetzt? Derzeit wird eine Datei als führend behandelt und die andere zeigt darauf — ob das die richtige Antwort bleibt, hängt davon ab, wie sich die Werkzeuge entwickeln.

**Q6** Braucht ein selbsttragendes Repository eine Aussage zur Aufbewahrung des Entscheidungsprotokolls? Es ist nach F5 keine Verständnisvoraussetzung mehr — aber es ist die Historie, und Historie zu verlieren ist ein anderer Verlust als Verständlichkeit zu verlieren.

## 10. Quellen & Verwandtes

*Zur Fertigstellung: Fundstellen prüfen und mit Abrufdatum versehen; die folgenden Angaben stammen aus der Entwurfsarbeit.*

**Quelle 1 · Architecture Decision Records (Michael Nygard, 2011, und die daraus entstandene Praxis).** Die verbreitete Form, Entscheidungen mit Kontext, Entscheidung und Konsequenzen festzuhalten. — *Wofür sie hier steht:* Der Ausgangspunkt von F5 — und zugleich die Stelle, an der dieses Konzept schärfer ist: Die verbreitete Praxis *verweist* auf den Entscheidungssatz, F5 holt Aussage und Begründung in das Dokument, das sie braucht.

**Quelle 2 · Verbreitete Konventionen für Agenten-Einstiegsdateien im Repositoriumswurzelverzeichnis.** — *Wofür sie hier steht:* Der Ort, an dem der Einstieg aus F2 tatsächlich gesucht wird; Gegenstand von Q5.

**Quelle 3 · Die Dossier-Reihe des Autors, in der dieses Dossier liegt.** — *Wofür sie hier steht:* Die laufende Anwendung aus §8, einschließlich der zwei Prüfläufe.

**Quelle 4 · Schwester-Konzept dieser Reihe: `self-describing-services`.** — *Wofür sie hier steht:* Dieselbe Denkfigur auf der Ebene laufender Dienste — eine Quelle statt verteilter Kopien —, wo dieses Dossier sie auf das Repository anwendet. Vertiefung, kein Verständnis-Muss.
