---
type: Proposal Rule
description: All modes read the same configuration with the same code and the check command is a mode that passes the start's judgment; precedence and secrets are imported from two sister concepts.
number: F5
binding: must
role: load-bearing
applies_to: [builder]
imports: [one-configuration-one-judgment, where-a-secret-comes-from]
---

# F5 · One configuration, one judgment

**Applies to:** builder. Imports `one-configuration-one-judgment` and `where-a-secret-comes-from`.

**What holds.** All modes read the same configuration with the same code; the check command is a mode and passes the judgment of the start: what it accepts starts, what it rejects does not. How the configuration is found, judged and supplied with secrets, precedence, the empty value, showing the source, the ranking of secret sources, is said by `one-configuration-one-judgment` and `where-a-secret-comes-from`; this concept presupposes both and does not repeat them. Their choices are decided with the user at adoption.

**Why.** A check that does not reach the same decision as the start is worse than none: it creates trust that breaks in the incident. That the check command is a mode of the same artifact is the reason it can pass the same judgment: it is the same code.

**Obvious and rejected.** *A separate schema validation tool*: the second truth about the configuration, see F2.

**How you notice it is violated.** The service does not start although the check was green.
