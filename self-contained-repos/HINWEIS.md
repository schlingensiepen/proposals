# Hinweis zur Fertigstellung — `self-contained-repos`

*Arbeitsdatei. Sie gehört zum Übergabestand und wird entfernt, sobald das Dossier auf 1.0
steht.*

## Stand

`PROPOSAL.de.md` liegt als **Materialfassung 0.9** vor: inhaltlich vollständig, F1–F7,
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

**Abschnitt 8, Bewährung.** Der Abschnitt beruft sich auf die Dossier-Reihe selbst als
laufendes Beispiel — und die war zum Zeitpunkt der Niederschrift **wenige Wochen alt**.
Das gehört ausgesprochen: Betriebserfahrung über längere Zeit fehlt. Der Beleg aus dem
Prüflauf mit dem kontextfreien Agenten ist stark, aber er ist eine Momentaufnahme und kein
Betrieb über Monate.

**Abschnitt 10, Quellen.** Als ungeprüft gekennzeichnet; vor der Veröffentlichung prüfen
und mit Abrufdatum versehen. **Quelle 3** verweist auf die Reihe, in der dieses Dossier
selbst liegt — dabei ist zu entscheiden, ob ein solcher Selbstbezug als Quelle taugt oder
besser in Abschnitt 8 gehört.

## Der besondere Fallstrick dieses Dossiers

Es beschreibt die Regel, nach der es selbst gebaut ist. **Jeder Verstoß darin ist doppelt
sichtbar**: Ein Dossier über „funktioniert es mit toten Links?", das im Fließtext auf
fremde Dokumente zeigt, widerlegt sich beim Lesen. Vor der Freigabe deshalb einmal
ausdrücklich die eigene Prüfregel auf das eigene Dokument anwenden.

## Beziehung zu anderen Dossiers

**Eng mit `two-repo-submodule-pattern` verbunden.** Dessen Kopierregel (F3) ist die
Anwendung der Inlining-Regel von hier (F5) auf den Übergang zwischen zwei Repositorien.
Beide verweisen aufeinander als **Vertiefung**, nie als Voraussetzung, und beide müssen
ohne das andere verständlich bleiben.

**Daraus folgt eine Wartungspflicht:** Wer an F5 hier etwas ändert, prüft F3 dort — und
umgekehrt. Diese Doppelung ist gewollt; sie zu übersehen ist der wahrscheinlichste
Folgefehler.
