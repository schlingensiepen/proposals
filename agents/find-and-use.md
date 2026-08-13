# Task: Find and Use a Dossier

You are looking for proven guidance, or a user pointed you at this repository.

## Find

1. Open the root `README.md` and scan the register (slug, type, status, one-liner).
2. Open the candidate's `PROPOSAL.en.md` (or `.de.md`) and read ONLY the frontmatter
   and the lines down to the abstract. That head exists precisely so you can decide
   whether the document deserves your context window. If `question` does not match your
   problem, stop and try the next candidate.

## Choose what to load

- You need **orders** (rules, procedure, checklist) → load the dossier's `AGENT.md`
  only. It is self-contained by design; you do not need the proposal.
- You need **reasoning, costs, limits, or discussion material** → load the PROPOSAL.
  English mirrors the German original; German leads on conflict (check the sync
  header).

## Use

- First check "Scope and limits" / "When not to use": preconditions, tipping
  conditions, non-goals. A dossier applied outside its scope is wrong with confidence.
- Respect the status: *Entwurf/Draft* and *Diskussion/Discussion* = not yet proven —
  say so to your user; *Bewährt/Proven* = has reference evidence; *Überholt/Superseded*
  = follow the successor pointer.
- A `Konzept` explains; it does not bind you. Only an `Implementierungskonzept` is
  normative (MUST/SHOULD/MAY sentences with requirement identifiers Fn).
- For a `Kompendium`, check `surveyed` and the regeneration rule. If observed behavior
  contradicts the compendium, trust the observation, then its primary sources — and
  report the drift.
- Cite findings and objections as `<slug> F3` / `<slug> Q2` — the numbers are stable
  across languages. Raise objections via issues/discussions addressed to Q numbers.
- Handle any credentials or tokens involved in applying a dossier according to that
  dossier's own security section; never quote secrets into conversations or files.
