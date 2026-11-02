---
status: draft              # draft | accepted | superseded — Werte englisch wie die Schlüssel
date: JJJJ-MM-TT           # Tag der Entscheidung, nicht des Schreibens
decision-makers: []        # wer entschieden hat — Rollen statt Klarnamen
consulted: []              # wer gefragt wurde
informed: []               # wer es wissen muss
supersedes: []             # Nummern, die dieses Dokument ersetzt — IMMER Liste, Nummern in Anführungszeichen: ["0011"]
superseded-by: []          # Nummern, die dieses Dokument ersetzt haben — IMMER Liste, Nummern in Anführungszeichen
---

<!-- VORLAGE: Kopieren, nach `<nnnn>-<kurztitel>.md` umbenennen, ausfüllen.
     Nicht: das zuletzt geschriebene Dokument als Muster nehmen — so vererben
     sich Abweichungen.

     KOPF-REGEL: In den Kopf gehört ausschließlich Buchhaltung. Keine
     Zusammenfassung, keine Begründung, keine Entscheidung. Der Kopf ist die
     einzige Stelle, die nach der Annahme noch geändert werden darf (Status,
     Beziehungslisten) — stünde dort Inhalt, wäre die Unveränderlichkeit des
     Inhalts praktisch aufgehoben.

     BEIDE BEZIEHUNGSFELDER SIND LISTEN, auch bei einem einzigen Eintrag: Eine
     Revision kann zwei Entscheidungen zusammenführen, eine Entscheidung in zwei
     zerfallen. Nummern in Anführungszeichen (["0011"]): ohne liest YAML 0011 als
     Zahl und verliert die führenden Nullen.

     DATEINAME: <nnnn>-<kurztitel>.md — vierstellige Nummer, Kleinbuchstaben,
     Bindestriche, Umlaute ausgeschrieben (zustaende, nicht zustände). Nach der
     Annahme wird der Name nicht mehr geändert; Nummern werden nie
     wiederverwendet, Lücken bleiben.

     BEISPIELE NUR MIT ERFUNDENEN WERTEN. Ein echter Schlüssel oder Hostname in
     einem angenommenen Dokument lässt sich nicht mehr still entfernen. -->

# ADR-<nnnn> · <Titel: die Entscheidung, nicht das Thema>

<!-- Der Titel beschreibt, was ENTSCHIEDEN wurde ("Datenhaltung bleibt SQLite"),
     nicht das Themenfeld ("Datenhaltung"). Er bleibt auch dann stehen, wenn das
     Dokument später abgelöst wird — sonst stehen zwei gleichnamige Einträge im
     Verzeichnis und Zitate werden mehrdeutig.

     WIRD DIESES DOKUMENT ABGELÖST, kommt hier — über "Kontext und Problem" —
     ein sichtbarer Vermerk hin, der auf den Nachfolger zeigt. Das ist neben dem
     Kopf die zweite und letzte Ausnahme von der Unveränderlichkeit: Der Kopf
     wird von Werkzeugen gelesen, dieser Vermerk von Menschen. -->

## Kontext und Problem

<Woraus die Frage entsteht. Was war der Anlass, was ging schief, was stand an?
Nicht: „Dieses Dokument behandelt X." Sondern: das Problem in der Sprache
dessen, der es hatte.>

## Betrachtete Optionen

<Knappe Liste zum Überfliegen; die gewählte markiert. Die ausgeschriebenen
Gründe stehen unten in „Verworfen und warum" — beide Abschnitte bedienen
verschiedene Leser und ersetzen einander nicht.>

- **<Option A>** (gewählt)
- **<Option B>** — verworfen
- **<Option C>** — verworfen

## Entscheidung

**Entscheidung:** <Ein Satz im Indikativ, zitierbar, ohne Begründung.>

**Begründung:** <Der Grund, nicht die Herleitung. Ohne ihn wird die Entscheidung
beim ersten Widerstand aufgegeben.>

**Abwägung:** <Was für die Gegenseite sprach und warum es nicht gereicht hat.
Wer hier nichts schreiben kann, hat die Alternative nicht ernst genommen.>

## Folgen

<Positiv und negativ, beides ausgeschrieben. Die negativen sind der Grund, warum
dieser Abschnitt Pflicht ist: Sie gelten im Moment der Entscheidung als
unwichtig und sind ein Jahr später das Einzige, was zählt.>

## Verworfen und warum

<Je verworfener Option ein Absatz mit ausgeschriebenem Grund. Dieser Abschnitt
ist der Wert des ganzen Bestands — er verhindert, dass die verworfene Lösung
wieder vorgeschlagen wird, und er ist der einzige Ort, an dem der Grund steht,
nachdem ihn alle vergessen haben. Steht eine Option oben in der Liste und fehlt
hier, ist unklar, ob sie geprüft oder vergessen wurde.

Wenn wirklich nichts zur Wahl stand: „Keine Alternative betrachtet, weil …" —
nie einfach weglassen.>

**<Option B>:** <…>

**<Option C>:** <…>
