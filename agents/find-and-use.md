# Task: find and use a proposal

You are looking for proven guidance, or a user pointed you at this repository.

## Find

1. Open the root `README.md` and scan the register: slug, type, status, one line each.
2. Open the candidate's `README.md`: question, thesis, identity card. If the question does not match your problem, stop and try the next candidate.
3. Open its `index.md`: one line per file. Decide from there what to load; do not load the whole bundle.

## Choose what to load

- You need **orders** (procedure, rules, checklist) → `AGENT.md`, plus the files in `choices/`, because the agent guide will send you there to decide the choices with your user.
- You need **reasoning, costs, limits or discussion material** → `concept.md`, the files in `rules/`, `scope.md`.
- You need **vocabulary** → `terms.md`.
- A proposal that `imports` another presupposes it: fetch that bundle from the same repository when the guide refers to it.

## Use

- First read `scope.md`: where it applies, when this no longer holds, what it does not solve, what is not yet settled. A proposal applied outside its scope is wrong with confidence.
- Respect the status from the register: *draft* means not yet proven; say so to your user. *stable* has reference evidence. *deprecated* names a successor in its `log.md`.
- Decide every choice with your user, never silently; record the decision where the guide says.
- A rule has one answer; if your case needs another, you are not using the concept, and you say so instead of bending the rule.
- For a compendium (previous form), check its `surveyed` date and regeneration rule; if observed behavior contradicts it, trust the observation, then its sources, and report the drift.
- Cite findings and objections as `<slug> F3` or `<slug> W1`; the numbers are stable.
- Handle any credentials or tokens involved according to the proposal's own security considerations; never quote secrets into conversations or files.
