---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with and the section names of the German template.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| MADR | *Markdown Any Decision Records*, a widespread open format for decision records; a Markdown file with a YAML head and fixed sections. | MADR | *Markdown Any Decision Records*, ein verbreitetes, offenes Format für Entscheidungsdokumente; Markdown-Datei mit YAML-Kopf und festen Abschnitten. |
| Decision record | one decision written down as one file in this format. | Entscheidungsdokument | eine Entscheidung, als eine Datei in diesem Format aufgeschrieben. |
| Head (frontmatter) | the YAML block between two lines of three hyphens at the start of the file. Machine-readable. | Kopf (Frontmatter) | der YAML-Block zwischen zwei Zeilen aus drei Bindestrichen am Dateianfang. Maschinenlesbar. |
| Status | the state of a record: draft, accepted, superseded. | Status | der Zustand eines Dokuments: Entwurf, angenommen, abgelöst. |
| Accepted | the state from which the decision applies. With it the content of the record becomes immutable: statement, rationale, options and consequences are not changed again. Exactly two places are excepted, the head (F3) and the opening of the document, where the supersession notice goes (F9), plus typographical errors and dead links. The reason: only this keeps it datable what applied when. If a record from 2024 contains a sentence from 2026, no statement in it can be placed in time, and therefore no rationale either, because a rationale always holds relative to a state of knowledge. Whoever wants to revise writes a new record that replaces the old one. | Angenommen | der Zustand, ab dem die Entscheidung gilt. Mit ihm wird der Inhalt des Dokuments unveränderlich: Aussage, Begründung, Optionen und Folgen werden nicht mehr geändert. Ausgenommen sind genau zwei Stellen, der Kopf (F3) und der Anfang des Dokuments, an den der Ablösungsvermerk kommt (F9), sowie Tippfehler und tote Verweise. Der Grund: Nur so bleibt datierbar, was wann galt. Steht in einem Dokument von 2024 ein Satz von 2026, ist keine Aussage darin mehr zeitlich verortbar, und damit auch keine Begründung, denn eine Begründung gilt immer gegenüber einem Kenntnisstand. Wer revidieren will, schreibt ein neues Dokument, das das alte ersetzt. |
| Superseded | the state of a record that a later record has replaced; its text stays as it was. | Abgelöst | der Zustand eines Dokuments, das ein späteres ersetzt hat; sein Text bleibt, wie er war. |
| Replacement relation | the link between a superseded record and the one replacing it; kept here in both directions, as the head fields `supersedes` and `superseded-by`. | Ersetzungsbeziehung | die Verbindung zwischen einem abgelösten Dokument und dem, das es ablöst; hier in beiden Richtungen geführt, als Kopffelder `supersedes` und `superseded-by`. |
| Supersession notice | the visible note below the heading of a superseded record, before its first section, pointing at the successor (F9). | Ablösungsvermerk | der sichtbare Vermerk unter der Überschrift eines abgelösten Dokuments, vor dem ersten Abschnitt, der auf den Nachfolger zeigt (F9). |
| Body | the entirety of a project's decision records. | Bestand | die Gesamtheit aller Entscheidungsdokumente eines Projekts. |
| Index | the overview file across the body, from which the standing of each record follows. | Verzeichnis | die Übersichtsdatei über den Bestand, aus der der Geltungsstand jedes Dokuments hervorgeht. |
| Standing | whether a record currently applies or has been superseded, and by which record. | Geltungsstand | ob ein Dokument gilt oder abgelöst ist, und durch welches. |
| Template | a shipped sample file copied to create a new record; this bundle ships one in `examples/`. | Vorlage | eine mitgelieferte Musterdatei, die für ein neues Dokument kopiert wird; dieses Bundle liefert eine in `examples/` mit. |
| Context and problem | the first mandatory section: where the question comes from (F5). | Kontext und Problem | der erste Pflichtabschnitt: woraus die Frage entsteht (F5). |
| Considered options | the brief list of alternatives with the chosen one marked (F5, F6). | Betrachtete Optionen | die knappe Liste der Alternativen, die gewählte markiert (F5, F6). |
| Decision | the section with one quotable sentence, the rationale and the trade-off (F5). | Entscheidung | der Abschnitt mit einem zitierbaren Satz, der Begründung und der Abwägung (F5). |
| Trade-off | what spoke for the other side and why it was not enough; part of the decision section. | Abwägung | was für die Gegenseite sprach und warum es nicht gereicht hat; Teil des Abschnitts Entscheidung. |
| Consequences | the section with the positive and negative effects of the decision (F5). | Folgen | der Abschnitt mit den positiven und negativen Wirkungen der Entscheidung (F5). |
| Rejected and why | the section spelling out the reason for each rejected option (F5, F6). | Verworfen und warum | der Abschnitt, der zu jeder verworfenen Option den Grund ausschreibt (F5, F6). |
| Writer | whoever writes or revises a record or keeps the body, a person or an agent. | Schreibender | wer ein Dokument schreibt, revidiert oder den Bestand pflegt, Mensch oder Agent. |
| rule | an entry with one answer; giving it up means no longer using the concept. | Festlegung | ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
| choice | an entry where the concept does not decide but names options with consequences; the adopter decides and records. This bundle has none yet. | Wahlpunkt | eine Stelle, an der das Konzept nicht entscheidet, sondern Optionen mit Folgen nennt; der Übernehmende entscheidet und hält fest. Dieses Bundle hat noch keinen. |
