---
type: Konzept
status: Entwurf
version: "0.9 · Materialfassung zur Fertigstellung"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: two-repo-submodule-pattern
language: de (führend)
question: "Wie trenne ich das, was die Öffentlichkeit sehen soll, von dem, was beim Entstehen anfällt — ohne zwei Wahrheiten zu bekommen und ohne die Entstehungsgeschichte zu verlieren?"
thesis: "Zwei Repositorien: eines für die Arbeit mit ihrer ganzen Historie, eines für das Erzeugnis mit einer Geschichte, die man vorzeigen kann — verbunden entweder als Submodul, wenn das Arbeits-Repository das Produkt-Repository einbettet, oder als zwei unabhängige Wurzeln mit einer Ignorier-Regel, wenn die verfügbaren Werkzeuge keine Submodul-Verweise schreiben können."
---

<!-- MATERIALFASSUNG. Enthält vollständig, was in dieser Arbeit zum Thema entschieden und
     erwogen wurde, einschließlich der verworfenen Wege — insbesondere die Werkzeug-Grenze,
     die zur zweiten Bauform geführt hat. Noch nicht durch die Endredaktion der Reihe:
     englische Fassung, AGENT.md, README und CHANGELOG fehlen. Konventionen in AGENTS.md
     und agents/create-or-maintain.md des Repositoriums. F- und Q-Nummern ab jetzt stabil. -->

# Arbeits-Repository und Produkt-Repository

**In einem Satz:** Was beim Entstehen anfällt — Entwürfe, Protokolle, verworfene Wege, Sitzungsnotizen — und was am Ende veröffentlicht wird, gehören in zwei verschiedene Repositorien mit zwei verschiedenen Historien; verbunden werden sie als Submodul oder, wo die Werkzeuge das nicht hergeben, als zwei unabhängige Wurzeln mit einer Ignorier-Regel.

**Kurzthese:** Wer ein Erzeugnis veröffentlicht, will eine vorzeigbare Geschichte: saubere Änderungen, keine Sackgassen, keine internen Notizen. Wer daran arbeitet, braucht das Gegenteil: jede Sackgasse, jede Begründung, jede Zwischenfassung. Beides in einem Repository führt entweder zu einer geschönten Historie oder zu einer Veröffentlichung, die mehr verrät als gewollt. Zwei Repositorien lösen das — und die interessante Frage ist nur noch, wie man sie verbindet, ohne zwei Wahrheiten zu bekommen.

## 1. Problem

Aus einer Zusammenarbeit — hier: zwischen dem Autor und einem KI-Agenten — fällt zweierlei an. Das eine ist das **Erzeugnis**: Code, Dokumentation, alles, was jemand anderes benutzen soll. Das andere ist die **Entstehung**: Entwurfsfassungen, Entscheidungsprotokolle mit Datum und Verlauf, verworfene Vorschläge samt Begründung, Notizen aus Arbeitssitzungen, Recherchen, die es nicht ins Ergebnis geschafft haben.

Legt man beides in ein Repository, muss man sich entscheiden, welche Seite leidet. Veröffentlicht man alles, geht die Entstehungsgeschichte mit an die Öffentlichkeit — samt Sackgassen, halbfertigen Gedanken und gelegentlich Dingen, die nur intern gemeint waren. Veröffentlicht man nichts, ist das Erzeugnis nicht teilbar. Und der beliebteste Ausweg — die Historie vor der Veröffentlichung glätten — vernichtet genau das, was die Entstehung wertvoll macht: die Begründungen und die verworfenen Wege.

Dazu kommt eine Beobachtung aus der Arbeit mit Agenten: Die Entstehung ist nicht Abfall, sondern **Arbeitsmaterial**. Ein Agent, der ein Vorhaben fortsetzt, braucht das Entscheidungsprotokoll dringender als der Mensch, weil er sich an nichts erinnert. Die Entstehung zu löschen heißt, dem nächsten Bearbeiter das Gedächtnis zu nehmen.

Drei Rollen tragen das Problem. **Der Autor** will beides behalten, aber nur eines zeigen. **Der Konsument** des Erzeugnisses will eine Historie sehen, die etwas über das Erzeugnis aussagt und nicht über Nachmittage. **Der Fortsetzende** — oft ein Agent — braucht die Entstehung vollständig, sonst beginnt er von vorn.

## 2. Das Konzept

Zwei Repositorien, zwei Zwecke, zwei Historien. Das **Arbeits-Repository** ist der Ort der Entstehung: Konzepte, Kurzspezifikationen, Entscheidungsprotokolle, Sitzungsstände, Recherchen, Planungen. Seine Historie darf unordentlich sein, denn sie ist ein Tagebuch. Das **Produkt-Repository** enthält ausschließlich das Erzeugnis in der Form, in der es jemand anderes bekommt; seine Historie ist eine Veröffentlichungsgeschichte.

Verbunden werden sie so, dass **eine Richtung führt**: Das Arbeits-Repository bettet das Produkt-Repository ein, nicht umgekehrt. Wer arbeitet, hat beides vor sich; wer nur das Erzeugnis will, klont nur dieses und sieht von der Werkstatt nichts. Für die Einbettung gibt es zwei Bauformen.

Die **Submodul-Bauform** ist die vollständige: Das Produkt-Repository liegt als Submodul in einem Unterverzeichnis des Arbeits-Repositoriums. Der Vorteil ist ein exakter Verweis — das Arbeits-Repository merkt sich, auf welchem Stand des Erzeugnisses ein bestimmter Arbeitsstand beruhte. Damit ist zu jedem Zeitpunkt der Entstehung rekonstruierbar, welches Erzeugnis dazu gehörte.

Die **Zwei-Wurzeln-Bauform** ist die genügsame: Zwei unabhängige Repositorien, das eine in einem Unterverzeichnis des anderen ausgecheckt, und das Unterverzeichnis steht in der Ignorier-Liste des äußeren. Es gibt dann keinen festgehaltenen Verweis mehr — dafür funktioniert alles mit Werkzeugen, die nur Dateien schreiben können. Genau dieser Fall trat in der Praxis ein und ist der Grund, warum diese Bauform hier gleichberechtigt steht (F5, §8).

Über beiden steht eine Regel, die wichtiger ist als die Wahl: **Nichts wandert unbemerkt vom einen ins andere.** Der Übergang von der Werkstatt zum Erzeugnis ist eine bewusste Handlung, und was dabei aus der Entstehung mitgenommen wird, wird nicht verlinkt, sondern mit Aussage und Begründung übernommen — sonst hängt das veröffentlichte Erzeugnis an einem Repository, das niemand sehen kann.

## 3. Begriffe

**Arbeits-Repository** — der Ort der Entstehung; enthält Entwürfe, Entscheidungsprotokolle, Notizen und Recherchen. In der Regel nicht öffentlich.

**Produkt-Repository** — der Ort des Erzeugnisses; enthält, was ein Konsument bekommt, und sonst nichts.

**Submodul** — ein Verweis eines Repositoriums auf einen bestimmten Stand eines anderen, festgehalten als eigener Eintragstyp in der Verzeichnisstruktur.

**Submodul-Verweis** — genau dieser Eintrag: ein Zeiger auf einen Stand des eingebetteten Repositoriums. Er ist kein Dateiinhalt, sondern ein besonderer Eintragstyp — was für die Werkzeugfrage in F5 entscheidend wird.

**Zwei-Wurzeln-Bauform** — zwei unabhängige Repositorien ohne festgehaltenen Verweis; das eine liegt im Arbeitsverzeichnis des anderen und wird dort ignoriert.

**Ignorier-Regel** — der Eintrag, der das eingebettete Verzeichnis vom äußeren Repository fernhält, damit es nicht versehentlich mit aufgenommen wird.

**Übergang** — die bewusste Handlung, mit der etwas aus der Werkstatt Teil des Erzeugnisses wird.

## 4. Festlegungen

**Landkarte.** Sechs Festlegungen. **F1** ist die Trennung selbst. **F2** legt die Richtung fest. **F3** regelt den Übergang zwischen beiden. **F4** und **F5** sind die zwei Bauformen mit ihren Bedingungen — F5 ist die Antwort auf eine Werkzeug-Grenze, die in der Praxis auftrat. **F6** betrifft die Ankündigungspflicht bei geteilter Arbeit. F1 trägt alles; F4 und F5 sind austauschbar, ohne dass der Rest fällt.

### F1 · Zwei Repositorien, zwei Historien

**Gilt für:** Autor.

**Was gilt.** Entstehung und Erzeugnis liegen in getrennten Repositorien. Die Historie des Arbeits-Repositoriums ist ein Tagebuch und darf unordentlich sein; die Historie des Produkt-Repositoriums ist eine Veröffentlichungsgeschichte und wird als solche geführt. Weder wird die eine geglättet noch die andere aufgebläht.

**Warum.** Die beiden Historien haben verschiedene Leser und verschiedene Zwecke, und jeder Versuch, sie in einer zu vereinen, opfert eine davon. Die Entstehung zu glätten vernichtet die Begründungen und die verworfenen Wege — also genau das, was einen späteren Bearbeiter davor bewahrt, dieselbe Sackgasse noch einmal zu betreten. Die Veröffentlichung mit Entstehungsverkehr zu füllen macht sie für Konsumenten unlesbar und gibt Interna preis, die niemand geprüft hat.

**Naheliegend und verworfen.** *Ein Repository, Entstehung in einem Unterverzeichnis, alles öffentlich* — verrät Interna und macht die Historie unlesbar; und es gibt keinen Weg zurück, wenn einmal veröffentlicht. *Ein Repository, Historie vor der Veröffentlichung glätten* — vernichtet die Entstehung dauerhaft; wer das tut, merkt den Verlust erst, wenn jemand fragt „warum eigentlich?". *Zwei Zweige statt zwei Repositorien* — Zweige teilen die Zugriffsrechte; man kann nicht einen veröffentlichen und den anderen zurückhalten. *Entstehung außerhalb der Versionsverwaltung, etwa in einem Wiki* — dann altert sie getrennt und ist beim Fortsetzen nicht zur Hand.

**Woran man merkt, dass es verletzt ist.** Vor einer Veröffentlichung wird Historie umgeschrieben — oder jemand zögert, etwas festzuhalten, weil es später sichtbar sein könnte.

### F2 · Die Arbeit umfasst das Erzeugnis, nie umgekehrt

**Gilt für:** Autor.

**Was gilt.** Das Arbeits-Repository bettet das Produkt-Repository ein. Das Produkt-Repository weiß von der Werkstatt nichts und enthält keinen Verweis auf sie.

**Warum.** Wer arbeitet, braucht beides gleichzeitig; wer nur benutzt, braucht nur eines. Die Einbettung in diese Richtung bedient beide, ohne dass eine Seite etwas mitschleppt. Umgekehrt wäre der Verweis nutzlos bis schädlich: Ein öffentliches Erzeugnis, das auf ein nicht öffentliches Repository zeigt, führt jeden Leser in eine Sackgasse — und verrät nebenbei dessen Existenz.

**Naheliegend und verworfen.** *Gegenseitige Verweise* — erzeugen einen Ringbezug und die Frage, welcher Stand zu welchem passt. *Ein Verweis aus dem Erzeugnis auf die Werkstatt „für Interessierte"* — Interessierte bekommen eine Fehlermeldung; wer die Entstehung zeigen will, veröffentlicht sie und behandelt sie damit als Erzeugnis. *Beide gleichrangig nebeneinander, ohne Einbettung* — funktioniert, verliert aber die Zusammengehörigkeit; man muss dann von Hand wissen, welche Verzeichnisse zusammen ausgecheckt gehören.

**Woran man merkt, dass es verletzt ist.** Im veröffentlichten Erzeugnis steht ein Verweis, dem niemand von außen folgen kann.

### F3 · Der Übergang ist eine bewusste Handlung — und er kopiert, statt zu verlinken

**Gilt für:** Autor.

**Was gilt.** Nichts gelangt beiläufig von der Werkstatt ins Erzeugnis. Was übernommen wird, wird **mit Aussage und Begründung** übernommen, nicht als Verweis auf ein Dokument der Werkstatt. Das Erzeugnis muss ohne jeden Zugriff auf das Arbeits-Repository vollständig verständlich sein.

**Warum.** Ein Verweis aus dem Erzeugnis in die Werkstatt ist für jeden Außenstehenden tot — und weil das Erzeugnis trotzdem vollständig aussieht, merkt es niemand. Die Übernahme mit Begründung kostet Schreibarbeit und ist der Preis dafür, dass das Veröffentlichte für sich steht. Und die Bewusstheit des Übergangs ist die einzige wirksame Kontrolle darüber, was nach außen geht: Wer beiläufig kopiert, kopiert irgendwann auch das, was nicht sollte.

**Naheliegend und verworfen.** *Dokumente aus der Werkstatt symbolisch verlinken oder einbinden* — sie sind für den Leser nicht erreichbar; die Verknüpfung sieht wie eine Lösung aus und ist keine. *Die Werkstatt-Dokumente einfach mitveröffentlichen* — dann ist die Trennung aufgehoben (F1). *Automatisch synchronisieren* — nimmt genau die Entscheidung weg, die den Wert des Übergangs ausmacht.

**Woran man merkt, dass es verletzt ist.** Ein Dokument im Erzeugnis verweist auf eines, das nur in der Werkstatt liegt — oder jemand kann nicht sagen, wann und warum etwas veröffentlicht wurde.

### F4 · Bauform Submodul: der exakte Verweis, wenn die Werkzeuge ihn hergeben

**Gilt für:** Autor.

**Was gilt.** Wo möglich, wird das Produkt-Repository als Submodul in ein Unterverzeichnis des Arbeits-Repositoriums eingebettet. Damit hält das Arbeits-Repository fest, auf welchem Stand des Erzeugnisses jeder Arbeitsstand beruhte.

**Warum.** Der festgehaltene Verweis beantwortet eine Frage, die sonst unbeantwortbar ist: Zu welchem Stand des Erzeugnisses gehörte diese Entscheidung? Wer ein halbes Jahr später ein Protokoll liest, kann den zugehörigen Stand auschecken, statt zu raten. Das ist der einzige echte Mehrwert gegenüber der genügsamen Bauform — und er ist beträchtlich, wenn die Entstehung tatsächlich als Gedächtnis benutzt wird.

**Naheliegend und verworfen.** *Ein Werkzeug, das mehrere Repositorien nebeneinander verwaltet* — löst dasselbe, bringt aber eine weitere Abhängigkeit und eine weitere Konfiguration mit; für zwei Repositorien unverhältnismäßig. *Alles in einem Repository mit Unterverzeichnissen* — genau das, was F1 verwirft. *Den Stand des Erzeugnisses im Protokoll notieren statt im Verweis* — funktioniert, ist aber Handarbeit und veraltet still.

**Woran man merkt, dass es verletzt ist.** Der Submodul-Verweis zeigt seit Monaten auf denselben Stand, obwohl das Erzeugnis sich weiterentwickelt hat — dann ist er kein Verweis mehr, sondern eine Behauptung.

### F5 · Bauform Zwei Wurzeln: die genügsame Form, wenn nur Dateien geschrieben werden können

**Gilt für:** Autor.

**Was gilt.** Können die verfügbaren Werkzeuge keine Submodul-Verweise schreiben, wird die Trennung als zwei unabhängige Wurzeln umgesetzt: Das eine Repository liegt in einem Unterverzeichnis des anderen und steht dort in der Ignorier-Liste. Es gibt dann keinen festgehaltenen Verweis; die Zusammengehörigkeit steht in den Einstiegsdokumenten beider Seiten, ausgeschrieben.

**Warum.** Diese Bauform ist keine Notlösung aus Bequemlichkeit, sondern die Antwort auf eine harte Werkzeug-Grenze, die in der Praxis auftrat. Ein Submodul-Verweis ist kein Dateiinhalt, sondern ein eigener Eintragstyp in der Verzeichnisstruktur — und Schnittstellen, die Repositorien über Dateioperationen bearbeiten, können Dateien anlegen und ändern, aber genau diesen Eintragstyp nicht schreiben. Wer ausschließlich über eine solche Schnittstelle arbeitet, kann ein Submodul weder anlegen noch seinen Verweis fortschreiben. Dann ist die genügsame Bauform ehrlicher als ein Submodul, das nie aktualisiert wird und dessen Verweis mit der Zeit lügt.

**Naheliegend und verworfen.** *Das Submodul trotzdem anlegen und den Verweis von Hand pflegen* — er veraltet, und ein veralteter Verweis ist schlechter als keiner, weil er Genauigkeit vortäuscht. *Auf die Trennung verzichten, weil die Verbindung nicht sauber geht* — wirft F1 weg, um F4 zu retten; die Trennung trägt auch ohne Verweis. *Das Erzeugnis in das Arbeits-Repository kopieren und von dort veröffentlichen* — erzeugt zwei Wahrheiten über dasselbe Erzeugnis, die auseinanderlaufen. *Auf ein anderes Werkzeug ausweichen* — legitim, aber oft nicht in der Hand dessen, der arbeitet; die Festlegung soll auch dann tragen.

**Woran man merkt, dass es verletzt ist.** Niemand kann sagen, welcher Stand des Erzeugnisses zu einem bestimmten Arbeitsstand gehört — und niemand vermisst es. (Das ist zugleich der Hinweis, dass F4 fehlt, nicht dass F5 falsch wäre.)

### F6 · Schreiben ins fremde Repository nur mit Ankündigung

**Gilt für:** alle Mitwirkenden, insbesondere Agenten.

**Was gilt.** Wo mehrere Beteiligte an denselben Repositorien arbeiten — etwa ein Mensch und ein Agent —, gilt eine ausdrückliche Regel dafür, wer wo ohne Rückfrage schreiben darf. Bewährt hat sich: Das Repository, das jemandem als Werkstatt zugewiesen ist, darf er frei beschreiben; jedes andere nur mit vorheriger Ankündigung dessen, was geschrieben wird.

**Warum.** Zwei Repositorien heißt zwei Vertrauensbereiche, und der Unterschied ist gerade der Punkt: In der Werkstatt ist ein Fehlgriff eine Zeile Historie, im Erzeugnis ist er veröffentlicht. Die Regel kostet nichts, wenn sie von Anfang an gilt, und sie erspart die unangenehme Rückfrage „was hast du da eigentlich geändert?".

**Naheliegend und verworfen.** *Volle Freiheit überall* — bequem, bis das erste Mal etwas Unfertiges im Erzeugnis steht. *Alles nur nach Freigabe* — macht die Zusammenarbeit zäh und verhindert genau die Selbstständigkeit, für die man einen Agenten hat. *Technisch trennen statt zu vereinbaren* — sinnvoll, wo es geht; wo derselbe Zugang beide Repositorien erreicht, bleibt nur die Vereinbarung.

**Woran man merkt, dass es verletzt ist.** Jemand entdeckt eine Änderung im Erzeugnis, von der er nichts wusste.

**Tragwerk (Kurzrekapitulation).** F1 trägt alles: Ohne die Trennung sind F2 bis F6 gegenstandslos. F2 gibt der Trennung ihre Richtung, F3 macht sie haltbar — ohne die Kopierregel hängt das Erzeugnis doch wieder an der Werkstatt. F4 und F5 sind Alternativen zueinander, nicht Ergänzungen; welche gilt, entscheidet die Werkzeuglage, und beide tragen F1 gleich gut. F6 ist unabhängig und gilt überall dort, wo mehrere an denselben Repositorien arbeiten.

## 5. Sicherheitsbetrachtung

| Risiko | Typischer Hergang | Gegenmaßnahmen |
|---|---|---|
| Interna wandern ins Erzeugnis | Beim Übergang wird ein Werkstattdokument mitkopiert, das nicht geprüft wurde | Der Übergang ist eine bewusste Handlung mit Durchsicht (F3); Werkstattinhalte werden übernommen, nicht übertragen |
| Existenz der Werkstatt wird preisgegeben | Ein Verweis im Erzeugnis zeigt auf ein nicht öffentliches Repository | Keine Verweise vom Erzeugnis zur Werkstatt (F2); ein toter Verweis verrät zugleich den Namen |
| Geheimnisse in der Werkstatt-Historie | Zugangsdaten landen in einem Entwurf und bleiben in der Historie | Die Trennung schützt davor nicht — sie begrenzt nur die Leserschaft. Geheimnisse gehören auch in ein nicht öffentliches Repository nicht; wird die Werkstatt je veröffentlicht, ist die Historie das Problem |
| Versehentliche Veröffentlichung des Falschen | Das eingebettete Verzeichnis wird vom äußeren Repository mit aufgenommen | Ignorier-Regel in der Zwei-Wurzeln-Bauform (F5); in der Submodul-Bauform sorgt der Eintragstyp selbst dafür |
| Unbemerkte Änderung im Erzeugnis | Ein Mitwirkender schreibt ohne Absprache | Ankündigungsregel (F6) |
| Falsche Zuordnung von Ständen | Ohne festgehaltenen Verweis wird der falsche Stand des Erzeugnisses zu einem Arbeitsstand angenommen | In der Submodul-Bauform durch den Verweis gelöst (F4); in der Zwei-Wurzeln-Bauform bewusst offen — dann gehört der Stand ins Protokoll geschrieben |

Grenze des Modells: Die Trennung schützt vor Versehen, nicht vor Absicht, und sie macht aus einem nicht öffentlichen Repository kein sicheres. Wer Zugriff auf die Werkstatt hat, sieht alles darin — die Trennung entscheidet nur, wer Zugriff braucht.

## 6. Was das kostet

**Zwei Repositorien sind zwei Repositorien (F1).** Zwei Rechteverwaltungen, zwei Klon-Vorgänge, zwei Historien, in denen man suchen muss, zwei Orte, an denen etwas liegen könnte. Für einen einzelnen Arbeitenden ist das wenig, für ein Team ist es Einarbeitungsaufwand.

**Der Übergang ist Handarbeit (F3).** Aussage und Begründung zu übernehmen statt zu verlinken dauert; und die Entscheidung, was hinübergeht, muss jemand treffen. Das ist gewollt, aber es ist Arbeit bei jeder Veröffentlichung.

**Redundanz zwischen Werkstatt und Erzeugnis (F3).** Was übernommen wurde, steht zweimal — und beide Fassungen können auseinanderlaufen, wenn die Werkstatt weiterarbeitet. Wer das nicht pflegt, hat im Erzeugnis eine veraltete Begründung stehen.

**Submodule sind eine Lernhürde (F4).** Wer sie nicht kennt, klont das Arbeits-Repository und findet ein leeres Verzeichnis. Das ist ein bekannter Stolperstein und kostet jeden Neuzugang einmal eine Verwirrung.

**Ohne Submodul fehlt die Zuordnung (F5).** Die genügsame Bauform kostet genau das, was F4 bringt: Man weiß nicht mehr, welcher Stand des Erzeugnisses zu einem Arbeitsstand gehörte, es sei denn, jemand schreibt es auf.

**Die Werkstatt wächst unbegrenzt (F1).** Weil ihre Historie ein Tagebuch ist, wird sie nie aufgeräumt. Das ist richtig so und heißt trotzdem, dass sie mit der Zeit unübersichtlich wird.

## 7. Geltung und Grenzen

### Randbedingungen

**Es gibt tatsächlich zwei Sorten Inhalt.** Wo alles, was entsteht, ohnehin veröffentlicht wird, ist die Trennung Aufwand ohne Ertrag.

**Die Rechteverwaltung erlaubt getrennte Sichtbarkeit.** Die ganze Konstruktion lebt davon, dass eines der beiden Repositorien zurückgehalten werden kann.

**Für die Submodul-Bauform: Werkzeuge, die Submodul-Verweise schreiben können.** Fehlt das, gilt F5 — und das ist kein Mangel, sondern der vorgesehene zweite Weg.

### Wann die Antwort kippt

**Die Entstehung soll selbst veröffentlicht werden.** Manche Vorhaben zeigen ihre Entstehung bewusst — dann gibt es keine zwei Vertrauensbereiche mehr, und die Trennung wird zur bloßen Ordnung, die man auch mit Unterverzeichnissen erreicht.

**Mehrere Erzeugnisse aus einer Werkstatt.** Bei drei oder vier eingebetteten Produkt-Repositorien wird die Konstruktion schwerfällig, und die Frage nach einem Werkzeug zur Mehrfachverwaltung stellt sich neu.

**Regeln verlangen Rückverfolgbarkeit über die Veröffentlichung hinaus.** Wo belegt werden muss, aus welcher Anforderung eine veröffentlichte Zeile stammt, reicht die Kopierregel aus F3 nicht — dann braucht es eine durchgehende Verknüpfung, und die widerspricht F2.

**Die Werkstatt wird zum Erzeugnis.** Wenn Entwürfe selbst das sind, was andere bekommen sollen — wie bei einer Dossier-Reihe —, dann ist das betreffende Repository ein Produkt-Repository und dieses Konzept auf es nicht anwendbar.

### Was es nicht löst

**Vertraulichkeit.** Ein nicht öffentliches Repository ist nicht verschlüsselt und nicht revisionssicher; die Trennung regelt Sichtbarkeit, nicht Schutz.

**Nachvollziehbarkeit von Anforderung zu Code.** Siehe oben — die Kopierregel erhält Begründungen, aber keine durchgehende Verknüpfung.

**Die Frage, was veröffentlicht gehört.** Das Konzept regelt, *wie* der Übergang geschieht, nicht *was* hinübergehört.

**Historienpflege im Erzeugnis.** Wie dort Änderungen gebündelt und benannt werden, ist ein anderes Thema.

## 8. Bewährung

**Angewendet im Vorhaben des Autors.** Für den Entwurfsablage-Dienst existieren zwei Repositorien: eines für die Entstehung — Konzept, Kurzspezifikation, Entscheidungsprotokoll, Planungen, Sitzungsstände — und eines für das Erzeugnis. Die Entstehung ist dabei nicht Beiwerk: Das Entscheidungsprotokoll wird tatsächlich als Gedächtnis benutzt, und zwar vor allem von Agenten, die an dem Vorhaben weiterarbeiten. Genau das ist der Grund, warum es nicht geglättet werden darf.

**Die Werkzeug-Grenze, aus der F5 entstand.** In der Praxis trat der Fall ein, dass die Arbeit an den Repositorien ausschließlich über eine Schnittstelle lief, die Dateien anlegen und ändern kann — Submodul-Verweise aber nicht, weil diese kein Dateiinhalt sind, sondern ein eigener Eintragstyp in der Verzeichnisstruktur. Erwogen wurde, das Submodul trotzdem anzulegen und den Verweis von Hand nachzuziehen; verworfen, weil ein Verweis, der nicht mitwandert, Genauigkeit vortäuscht, die er nicht hat. Die Entscheidung fiel auf zwei unabhängige Wurzeln mit einer Ignorier-Regel für das eingebettete Verzeichnis — und der Vorschlag dazu kam vom Autor, nachdem die Grenze benannt war. Aus dieser Episode stammt die Formulierung von F5, einschließlich der Begründung, warum sie keine Notlösung ist.

**Die Ankündigungsregel aus F6** entstand aus derselben Zusammenarbeit: Ein Repository wurde dem Agenten als freie Werkstatt zugewiesen, das andere durfte er nur mit vorheriger Ankündigung anfassen. Die Regel hat sich gehalten; ein Fall, in dem sie unnötig gebremst hätte, ist nicht aufgetreten.

*Zur Fertigstellung: Betriebserfahrung über längere Zeit fehlt, insbesondere zur Frage, wie gut sich die Redundanz aus F3 auf Dauer pflegen lässt. Ob die Submodul-Bauform (F4) im selben Vorhaben je zum Einsatz kam, ist im Abschnitt ehrlich zu benennen — nach derzeitigem Stand wurde sie erwogen und zugunsten von F5 nicht umgesetzt.*

## 9. Offene Fragen

**Q1** *(Auftrag an den Verwender)* Wer darf in welchem der beiden Repositorien ohne Ankündigung schreiben (F6)? Die Antwort hängt an der Zusammensetzung der Mitwirkenden; das Konzept verlangt nur, dass sie ausdrücklich getroffen und aufgeschrieben ist.

**Q2** *(Auftrag an den Verwender)* Wie wird in der Zwei-Wurzeln-Bauform festgehalten, welcher Stand des Erzeugnisses zu einem Arbeitsstand gehört (F5) — gar nicht, im Protokolleintrag, über Veröffentlichungsmarken? Ohne Antwort geht diese Information verloren.

**Q3** Sollte die Reihenfolge umkehrbar sein — also das Produkt-Repository die Werkstatt einbetten, wenn die Werkstatt ebenfalls öffentlich ist? F2 verbietet es aus guten Gründen für den Regelfall; für den Sonderfall „beides öffentlich" ist es nicht durchdacht.

**Q4** Wie geht man mit mehreren Erzeugnissen aus einer Werkstatt um? Ab welcher Zahl lohnt ein Werkzeug zur Mehrfachverwaltung, und verändert das die Festlegungen oder nur die Handhabung?

**Q5** Braucht der Übergang aus F3 eine Prüfliste — etwa: enthält das Übernommene Namen, Zugangswege oder Aussagen über Dritte? Derzeit ist er eine bewusste Handlung ohne festgelegte Prüfung.

**Q6** Wie verhält sich das Muster zu Veröffentlichungsverfahren, die aus der Werkstatt heraus bauen und veröffentlichen? Dann wandert nicht mehr nur Wissen, sondern ein erzeugtes Artefakt — und die Frage, wo dessen Bauleitung wohnt, ist offen.

## 10. Quellen & Verwandtes

*Zur Fertigstellung: Fundstellen prüfen und mit Abrufdatum versehen.*

**Quelle 1 · Dokumentation zu Submodulen in der verbreiteten Versionsverwaltung.** — *Wofür sie hier steht:* Die Mechanik von F4, und die Erklärung, warum ein Submodul-Verweis ein eigener Eintragstyp ist — die Tatsache, an der die Werkzeug-Grenze in F5 hängt.

**Quelle 2 · Die Schnittstellendokumentation des verwendeten Hosting-Anbieters zu Dateioperationen.** — *Wofür sie hier steht:* Der Beleg für die Grenze aus F5: Dateien lassen sich anlegen und ändern, Submodul-Verweise nicht.

**Quelle 3 · Entwurfsablage-Dienst des Autors samt zugehörigem Arbeits-Repository.** — *Wofür sie hier steht:* Die Anwendung aus §8. *Hinweis für die Fertigstellung: Beide Repositorien sind derzeit nicht öffentlich; entweder sie werden es, oder der Eintrag trägt einen entsprechenden Vermerk.*

**Quelle 4 · Schwester-Konzept dieser Reihe: `self-contained-repos`.** — *Wofür sie hier steht:* Die Regel, dass Wissen mit Aussage und Begründung übernommen und nicht verlinkt wird, ist dort ausführlich begründet; F3 wendet sie auf den Übergang zwischen zwei Repositorien an. Vertiefung, kein Verständnis-Muss.
