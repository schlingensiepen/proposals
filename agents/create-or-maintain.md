# Task: Create or Maintain a Dossier

You create a new dossier (Konzept or Kompendium) or maintain an existing one (revision,
translation sync, status change). Discuss content with the author before writing —
dossiers present the author's proven concepts, not invented ones.

## Choose the type

- **Konzept** — a thesis with decisions: copy `_template/konzept/` (10 sections,
  four-field decision blocks F1…).
- **Kompendium** — curated factual/operational knowledge: copy `_template/kompendium/`
  (8 sections, uniform per-subject blocks, `surveyed` date, regeneration rule).
- **Implementierungskonzept** — a self-contained description of a system to be built:
  copy `_template/konzept/` and apply three reinterpretations ("what holds" becomes a
  MUST/SHOULD/MAY sentence and Fn the requirement identifier; "scope and limits" names
  the non-goals; "track record" becomes "implementation status").

## Create

1. Copy the template directory to a new slug: lowercase English, hyphens, no numbers
   (e.g. `cp -r _template/konzept my-new-slug`).
2. Write `PROPOSAL.de.md` first — German is the leading version. Start with the
   frontmatter: if `thesis` does not fit into one sentence, the scope is not cut yet.
3. Follow the embedded comments in the template — they are the format rules. Sections
   are never dropped: an empty one says „entfällt, weil …". The security section is
   mandatory. References go into the sources section only; the dossier must work with
   every link dead.
4. Write `PROPOSAL.en.md` with identical structure and identical F/Q numbers; set the
   sync header ("Translated from the German original · sync: vX.Y").
5. Write `AGENT.md` strictly self-contained: every rule and checklist item in full
   text; F numbers only as provenance markers in parentheses. An agent must be able to
   act correctly without ever opening the proposal.
6. Fill the dossier `README.md` (bilingual thesis) and `CHANGELOG.md` (first entry).
7. Register the dossier in the root `README.md`, section "## Register". The register is
   a list of prose entries, not a table — copy the form of the entries already there:
   bold link to the directory · Typ/Type · Status · version and date (Kompendium: plus
   `surveyed: YYYY-MM`) — em dash — one bilingual sentence naming what the dossier
   answers. Entries are ordered from general to specific: a dossier that generalizes
   another stands above it, and an entry that applies another says so in its sentence.
   Slugs that are merely intended stay in the "Kandidaten / Candidates" line below the
   entries; remove a slug from that line when its dossier lands.

## Maintain

- Any content change to `PROPOSAL.de.md`: bump the version, add a CHANGELOG entry, and
  in the same change either update the English version or set its header to
  "sync: pending (de vX.Y)". Lagging is allowed; invisible lagging is not.
- The root README register entry belongs to the dossier head: whenever version, status,
  `surveyed`, or the one-line summary changes, the entry changes in the same commit.
  A register that contradicts a dossier head is the drift this series exists to avoid.
- `AGENT.md` follows the proposal. Review question for every change: does AGENT order
  what PROPOSAL justifies — and is every Fn either covered or deliberately out of scope?
- Status transitions (Entwurf → Diskussion → Bewährt → Überholt) only via CHANGELOG;
  "Bewährt" requires reference evidence in the track-record section; "Überholt"
  requires a successor pointer. Nothing is reinterpreted retroactively.
- Never renumber F or Q items; retired numbers stay retired.
- For a Kompendium: re-survey when its regeneration rule fires; update `surveyed` and
  the CHANGELOG.
- When a dossier states a fact about another dossier ("in preparation", "not yet
  published"), that sentence ages too: publishing the referenced dossier is also a
  maintenance event for the referring one.

## Checks before committing

- [ ] Frontmatter parses; `question`/`thesis` present (Kompendium: also `surveyed`).
- [ ] Section numbering identical in de and en; F/Q numbers identical.
- [ ] Every decision block has all four fields (or an explicit „entfällt, weil …").
- [ ] No link leaves the dossier directory except https links in the sources section.
- [ ] No references to internal documents of other projects; attribution "the author".
- [ ] Root README register entry matches the dossier head (version, status, sync,
      `surveyed`), and the slug is gone from the candidates line.
