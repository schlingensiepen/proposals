# Hinweis zur Fertigstellung — `two-repo-submodule-pattern`

*Arbeitsdatei. Sie gehört zum Übergabestand und wird entfernt, sobald das Dossier auf 1.0
steht.*

## Stand

`PROPOSAL.de.md` liegt als **Materialfassung 0.9** vor: inhaltlich vollständig, F1–F6,
Q1–Q6, alle zehn Abschnitte besetzt. **Nicht freigegeben** — die Durchsicht durch den Autor
steht aus, erst danach 1.0.

## Was noch fehlt

| | |
|---|---|
| `PROPOSAL.en.md` | gleiche Struktur, gleiche F- und Q-Nummern, Kopfvermerk `sync: v1.0` |
| `AGENT.md` | englisch, Skill-Frontmatter, **strikt selbsttragend** — F-Nummern dort nur als Herkunftsvermerk |
| `README.md` · `CHANGELOG.md` | nach dem Muster der veröffentlichten Dossiers |
| Register | Prosa-Eintrag im Wurzel-`README.md`, Slug aus der Kandidatenzeile entfernen |

## Was nur der Autor liefern kann

**Abschnitt 10, Quelle 3.** Sie verweist auf den Entwurfsablage-Dienst und sein
Arbeits-Repository — **beide sind derzeit nicht öffentlich**. Zwei Wege: entweder sie
werden es bis zur Veröffentlichung, oder der Eintrag trägt einen ausdrücklichen Vermerk,
dass die Quelle nicht einsehbar ist. Ein Verweis, dem niemand folgen kann, ohne dass es
dabeisteht, ist schlechter als keiner.

**Abschnitt 10 im Übrigen.** Fundstellen prüfen und mit Abrufdatum versehen; im Zweifel
streichen statt behaupten.

## Was beim Fertigstellen leicht schiefgeht

**Die zwei Bauformen sind nicht zwei Meinungen.** F4 (Submodul) und F5 (zwei Wurzeln)
beschreiben denselben Zweck unter verschiedenen Werkzeugbedingungen — F5 ist die genügsame
Form für den Fall, dass die vorhandenen Werkzeuge keine Submodul-Verweise schreiben können.
Beim Übersetzen und beim Schreiben der `AGENT.md` darf daraus keine Wahlfreiheit nach
Geschmack werden: Die Bedingung entscheidet, nicht die Vorliebe.

**F5 stammt aus einem echten Vorfall**, nicht aus einer Überlegung (Abschnitt 8, „Die
Werkzeug-Grenze, aus der F5 entstand"). Diese Herkunft trägt die Festlegung — sie sollte
beim Kürzen nicht verlorengehen.

## Beziehung zu anderen Dossiers

**Eng mit `self-contained-repos` verbunden.** Die Kopierregel hier (F3) ist die Anwendung
der Inlining-Regel von dort (F5) auf den Übergang zwischen zwei Repositorien. Beide
verweisen aufeinander als **Vertiefung**, nie als Voraussetzung, und beide müssen ohne das
andere verständlich bleiben.

**Daraus folgt eine Wartungspflicht:** Wer an F3 hier etwas ändert, prüft F5 dort — und
umgekehrt.
