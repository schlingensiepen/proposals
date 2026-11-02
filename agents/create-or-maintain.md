# Task: create or maintain a proposal

You create a new bundle or maintain an existing one (revision, status change, moving a dossier of the previous form into bundle form). Discuss content with the author before writing: proposals present the author's proven concepts, not invented ones.

## The form

Copy [`_template/proposal/`](../_template/proposal/) to a new slug (lowercase English, hyphens, no numbers). The template is the complete skeleton; the root README's section "Layout of a bundle" says what each file holds. Every Markdown file except `index.md` and `log.md` carries a YAML head with `type` and a one-sentence `description`; `AGENT.md` also carries `name: <slug>` so that skill loaders find it; files in `examples/` that are meant to be copied as templates keep their own head instead.

## Create

1. Start with `README.md`: question and thesis in one sentence each. If the thesis will not fit into one sentence, the scope is not cut yet.
2. Write `concept.md` (problem, the concept, the procedure) and the rules in `rules/`, one file each, four fields each: *what holds*, *why*, *considered and rejected*, *how you notice it is violated*. A field that does not apply says "does not apply, because …".
3. Sort out the choices. A choice is a point where more than one answer is legitimate and the right one depends on the adopter's case. Test: does every option have a use case in which it is right? Then it is a choice, one file in `choices/`, with options, consequences and when each fits; the proposal does not decide. Only one option is right: a rule. No option is right: it goes under "considered and rejected". Converting a rule into a choice, or the reverse, is the author's decision, not yours.
4. Write `scope.md` (security considerations, costs, where it applies, when this no longer holds, what it does not solve, what is not yet settled), `evidence.md`, `sources.md` (imports, related proposals by slug, external sources with URLs) and `terms.md` (every term of the bundle, English and German, each with a short explanation).
5. Write `AGENT.md` self-contained: every step, rule and checklist item in full; rule numbers only in parentheses as provenance. It must contain the step in which the agent decides every choice with the user and records the decision.
6. Write `index.md`: every file with one line, grouped (start here · rules · choices · scope and evidence · adopting · history). Let `README.md` describe the same groups briefly with links and end with the two sentences for reader and agent. Write `log.md` with the creation entry.
7. Leave `used-by.md` and `extensions.md` as the template has them; they are generated from the register.
8. Register the bundle in the root `README.md` under "Bundles": bold link, type, status, version with date, em dash, one sentence saying what it answers. Entries are ordered from general to specific.

## Maintain

- Any content change bumps the version and adds an entry to `log.md`. A changed rule keeps its number; a retired rule's number stays retired; a new rule takes the next free number.
- The register line in the root README belongs to the bundle's `README.md` head: whenever version, status or the one-line summary changes, the register changes in the same commit.
- `AGENT.md` follows the rules and choices. Review question for every change: does AGENT order what the rules justify, is every rule either covered or deliberately out of scope, and is every choice put to the user?
- Status transitions (draft → stable → deprecated) only through `log.md`; "stable" requires reference evidence in `evidence.md`; "deprecated" requires a successor named in `log.md`.
- Never state another bundle's status, version or content inside a bundle; name it by slug.
- Moving a dossier of the previous form into bundle form: the content is the author's and stays; you sort it into the files of the template, replace section and question references by bundle references, turn the old "open questions" into duties of adoption (in `AGENT.md`), limits ("not yet settled" in `scope.md`) or candidates for choices (to be decided by the author), and drop `PROPOSAL.de.md`, `PROPOSAL.en.md`, `CHANGELOG.md` (its entries go to `log.md`).

## Checks before committing

- [ ] `README.md` head complete: type, description, version, date, status, author, extends, imports.
- [ ] Every rule file has the four fields; numbers in file names match the heads.
- [ ] Every choice file names options, consequences and conditions, and ends with "record your decision".
- [ ] `index.md` lists every file exactly once; all links relative and resolving inside the bundle.
- [ ] No status, version or rule numbers of other bundles inside this one.
- [ ] No internal hostnames, paths, session links or secrets; no personal names in the text (the `author` field in the head and authors of cited works are allowed); repositories that are not public are marked as such.
- [ ] `terms.md` covers every term used in rules, choices and concept.
- [ ] The root README register line matches the bundle head.
