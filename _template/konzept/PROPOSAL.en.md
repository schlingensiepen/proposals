---
type: Concept
status: Draft
version: "0.1"
date: YYYY-MM-DD
author: the author
slug: <slug>
language: en — translated from the German original
sync: v0.1 (YYYY-MM-DD)
question: "<the one question a reader brings — phrased as a question>"
thesis: "<the answer in one sentence, quotable>"
---

<!-- HEAD RULE: The first lines decide whether a reader — or an agent — loads the whole
     document into its context window. question + thesis above and the abstract right
     below the title must suffice for that decision. If the thesis does not fit into one
     sentence, the scope is not cut yet.
     SYNC RULE: German is the leading version. If the German original moves ahead, set
     the sync line above to "sync: pending (de vX.Y)" instead of silently drifting.
     F and Q numbers are identical across languages. -->

# <Title>

**In one sentence:** <the thesis spelled out — what holds, without justification>

**Abstract:** <≤ 3 sentences: problem → concept → gain. Nobody needs more preview.>

<!-- STRUCTURE RULE: Ten sections, fixed order, none omitted. If one is empty, it says
     "omitted, because …" — an omitted section looks like a forgotten one; a justified
     empty one is itself a statement.
     REFERENCE RULE: References (including sister dossiers) appear exclusively in
     section 10. A dossier that points at external documents mid-text stops being
     self-contained at exactly that spot. Knowledge from sources (ADRs, internal
     documents) is pulled in with statement AND rationale, never referenced — the
     dossier must remain understandable and followable with every link dead. -->

## 1. Problem

<Where the question comes from, in the language of the person asking it — as a problem,
not a topic announcement.>

## 2. The concept

<The core in 5–8 sentences that can be retold without the document at hand. Mechanism
only — justifications and exceptions belong to section 4.>

## 3. Terms

<Every term the dossier uses that is not common language — one definition line each,
including the seemingly obvious ones: those are exactly the ones nobody explains, and
outside the project they are not obvious. If empty: "omitted, because the dossier works
with common language.">

## 4. Decisions

<!-- The heart of the document. One uniform block per decision, numbered F1, F2, …;
     F numbers are stable across languages and citable ("<slug> F3"). Four fields,
     always the same, always in this order — documents like this are read crosswise
     ("what was actually rejected?"), which only works if the same answer always sits
     in the same place. In the implementation-concept type, "What holds" becomes a
     normative MUST/SHOULD/MAY sentence; Fn is then the requirement identifier. -->

### F1 · <descriptive short title>

**What holds.** <One indicative sentence, no justification — quotable without dragging
the paragraph along.>

**Why.** <The reason, not the derivation. Without it, the decision is abandoned at the
first pushback.>

**Obvious and rejected.** <The alternatives a reader would propose themselves, each with
the reason for rejection. The most expensive field to lack: whoever does not read it
proposes the rejected solution again — and nobody remembers the reason. If truly nothing
suggests itself: "omitted, because …".>

**Symptom of violation.** <The operational symptom, not the rule restated. Turns an
opinion into a checkable statement — whoever cannot name a symptom may not have a
decision, but a taste.>

### F2 · <…>

<!-- … further decisions, same block … -->

**Load-bearing structure.** <Which decision rests on which — and what falls if one falls.
Answers every adopter's question: what can I leave out? Example: "If F1 falls, F3 and F4
lose their purpose; F5 stands independently.">

## 5. Security considerations

<Mandatory: threats, leak channels, countermeasures — ideally as a table
attack · typical course · countermeasure. Name the limit too: what does the concept NOT
protect against? "omitted, because …" only with a solid justification.>

## 6. What it costs

<The drawbacks, spelled out and not relativized — no "but in return we gain …" in the
same sentence. A concept without named costs reads like advertising and is believed
accordingly: not at all. Whoever knows the costs can later tell whether they materialized
or whether a different problem is at hand.>

## 7. Scope and limits

<Three questions, three brief answers:
– Preconditions: what must the environment provide for the answer to hold?
– Tipping conditions: which changed condition makes the answer wrong? A handed-on dossier
  is read in a different situation — these lines are the only protection against someone
  adopting the answer into an environment where it is false.
– Not solved: which neighboring questions does the dossier NOT answer — so nobody
  silently assumes they are covered?>

## 8. Track record

<Reference implementations and operational experience — and ONE continuous example,
marked as such: the same case from start to finish, not three sketched ones; where it
helps, with the actual wording of a request, a response, or a configuration line. The
status "Proven" requires this section. Until something exists: "omitted, because no
reference exists yet (status: Draft)".>

## 9. Open questions

<Numbered Q1, Q2, … — stable across languages so discussion stays addressable. Accepted
objections become a new version plus a changelog entry.>

**Q1** <…>

## 10. Sources & related work

<Evidence, standards, related work — the only place for references. Sister dossiers only
as deepening, never as prerequisite.>
