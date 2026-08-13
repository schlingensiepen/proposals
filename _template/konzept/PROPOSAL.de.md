---
type: Konzept
status: Entwurf
version: "0.1"
date: JJJJ-MM-TT
author: der Autor
slug: <slug>
language: de (führend)
question: "<Die eine Frage, die ein Leser mitbringt — als Frage formuliert>"
thesis: "<Die Antwort in einem Satz, zitierfähig>"
---

<!-- KOPF-REGEL: Die ersten Zeilen entscheiden, ob ein Leser — oder ein Agent — das ganze
     Dokument in sein Kontextfenster lädt. question + thesis oben und die Kurzthese direkt
     unter dem Titel müssen dafür genügen. Wer die thesis nicht in einen Satz bekommt, hat
     den Zuschnitt noch nicht. (Frontmatter-Schlüssel englisch, Werte in Dokumentsprache.) -->

# <Titel>

**In einem Satz:** <die These ausformuliert — was gilt, ohne Begründung>

**Kurzthese:** <≤ 3 Sätze: Problem → Konzept → Gewinn. Mehr Vorschau braucht niemand.>

<!-- GLIEDERUNGS-REGEL: Zehn Abschnitte, feste Reihenfolge, keiner wird weggelassen. Ist
     einer leer, steht dort „entfällt, weil …" — ein weggelassener Abschnitt sieht aus wie
     ein vergessener; ein begründet leerer ist selbst eine Aussage.
     VERWEIS-REGEL: Verweise (auch auf Schwester-Dossiers) stehen ausschließlich in
     Abschnitt 10. Ein Dossier, das im Fließtext auf externe Dokumente zeigt, ist an genau
     dieser Stelle nicht mehr geschlossen. Wissen aus Quellen (ADRs, interne Dokumente)
     wird mit Aussage UND Begründung hierher übernommen, nie referenziert — das Dossier
     muss auch mit toten Links verständlich und befolgbar bleiben. -->

## 1. Problem

<Woraus die Frage entsteht, in der Sprache dessen, der sie stellt — als Problem, nicht als
Themenansage. Nicht: „Dieses Dokument behandelt Authentifizierung." Sondern: „Mein Dienst
wird von Agenten benutzt, und alles, was ein Agent liest, kann in fremde Hände geraten.">

## 2. Das Konzept

<Der Kern in 5–8 Sätzen, die man weitererzählen kann, ohne das Dokument zur Hand zu haben.
Nur der Mechanismus — Begründungen und Ausnahmen kommen in Abschnitt 4.>

## 3. Begriffe

<Jeder Begriff, den das Dossier benutzt und der nicht allgemein ist — eine Zeile Definition
pro Begriff. Auch die scheinbar selbstverständlichen: Gerade sie vergisst man zu erklären,
und außerhalb des Projekts sind sie es nicht. Wenn leer: „entfällt, weil das Dossier mit
allgemeiner Sprache auskommt.">

## 4. Festlegungen

<!-- Das Herzstück. Je Festlegung ein gleichförmiger Block, nummeriert F1, F2, …; die
     F-Nummern sind sprachübergreifend stabil und zitierbar („<slug> F3"). Vier Felder,
     immer dieselben, immer in dieser Reihenfolge — man liest solche Dokumente quer
     („was wurde eigentlich verworfen?"), und das geht nur, wenn dieselbe Auskunft immer
     an derselben Stelle steht. Beim Typ Implementierungskonzept wird „Was gilt" zum
     normativen MUSS/SOLL/KANN-Satz; die Kennung Fn ist dann die Anforderungs-Kennung. -->

### F1 · <sprechender Kurztitel>

**Was gilt.** <Ein Satz im Indikativ, ohne Begründung — zitierbar, ohne den Absatz
mitzuschleppen.>

**Warum.** <Der Grund, nicht die Herleitung. Ohne ihn wird die Festlegung beim ersten
Widerstand aufgegeben.>

**Naheliegend und verworfen.** <Die Alternativen, die ein Leser selbst vorschlagen würde,
je mit dem Grund der Verwerfung. Das teuerste Feld, wenn es fehlt: Wer es nicht liest,
schlägt die verworfene Lösung wieder vor — und niemand erinnert den Grund. Wenn wirklich
nichts naheliegt: „entfällt, weil …".>

**Woran man merkt, dass es verletzt ist.** <Das Symptom im Betrieb, nicht die Regel.
Macht aus einer Meinung eine prüfbare Aussage — wer kein Symptom nennen kann, hat
womöglich keine Festlegung, sondern einen Geschmack.>

### F2 · <…>

<!-- … weitere Festlegungen, gleicher Block … -->

**Tragwerk.** <Welche Festlegung ruht auf welcher — und was fällt, wenn eine fällt. Das
beantwortet die Frage jedes Übernehmenden: Was kann ich weglassen? Beispiel: „Fällt F1,
verlieren F3 und F4 ihren Zweck; F5 bleibt unabhängig bestehen.">

## 5. Sicherheitsbetrachtung

<Pflichtteil: Bedrohungen, Leck-Kanäle, Gegenmaßnahmen — gern als Tabelle
Angriff · typischer Hergang · Gegenmaßnahme. Auch die Grenze benennen: Wogegen hilft das
Konzept NICHT? „entfällt, weil …" nur mit tragfähiger Begründung.>

## 6. Was das kostet

<Die Nachteile, ausgeschrieben und nicht relativiert — kein „dafür gewinnen wir …" im
selben Satz. Ein Konzept ohne genannte Kosten liest sich wie Werbung und wird entsprechend
geglaubt: nämlich nicht. Wer die Kosten kennt, erkennt später, ob sie eingetreten sind
oder ob er ein anderes Problem hat.>

## 7. Geltung und Grenzen

<Drei Fragen, drei knappe Antworten:
– Randbedingungen: Was muss die Umgebung hergeben, damit die Antwort gilt?
– Kipp-Bedingungen: Welche geänderte Bedingung macht die Antwort falsch? Ein
  weitergegebenes Dossier wird in einer anderen Lage gelesen — diese Zeilen sind der
  einzige Schutz davor, dass jemand die Antwort in eine Umgebung übernimmt, in der sie
  nicht stimmt.
– Nicht gelöst: Welche Nachbarfragen beantwortet das Dossier NICHT — damit niemand still
  annimmt, sie seien mitgedeckt?>

## 8. Bewährung

<Referenz-Implementierungen und Betriebserfahrung — und EIN durchgehendes, als solches
gekennzeichnetes Beispiel: derselbe Fall von Anfang bis Ende, nicht drei angerissene;
wo es hilft, mit tatsächlichem Wortlaut von Anfrage, Antwort oder Konfigurationszeile.
Der Status „Bewährt" setzt diesen Abschnitt voraus. Solange nichts vorliegt:
„entfällt, weil noch keine Referenz existiert (Status: Entwurf)".>

## 9. Offene Fragen

<Nummeriert Q1, Q2, … — sprachübergreifend stabil, damit Diskussion adressierbar wird.
Angenommene Einwände werden neue Version + CHANGELOG-Eintrag.>

**Q1** <…>

## 10. Quellen & Verwandtes

<Belege, Standards, verwandte Arbeiten — der einzige Ort für Verweise. Schwester-Dossiers
nur als Vertiefung, nie als Voraussetzung.>
