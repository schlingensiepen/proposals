---
type: Compendium
status: Draft
version: "0.1"
date: YYYY-MM-DD
author: the author
slug: <slug>
language: en — translated from the German original
sync: v0.1 (YYYY-MM-DD)
question: "<the one lookup question this compendium answers>"
thesis: "<the core answer in one sentence>"
surveyed: YYYY-MM
---

<!-- HEAD RULE: as in the concept template — the first lines (question, thesis) decide
     whether a reader or agent loads the whole document. Additionally mandatory for a
     compendium: `surveyed` (when the facts were gathered). A compendium justifies no
     design decisions; it answers lookup questions — and therefore ages: the survey date
     and the regeneration rule (section 6) are its best-before date.
     STRUCTURE AND REFERENCE RULE: as in the concept template — eight fixed sections,
     none omitted ("omitted, because …"), references exclusively in the sources section,
     external knowledge pulled in with statement and origin, never merely referenced.
     SYNC RULE: German is leading; if it moves ahead, set sync to "pending (de vX.Y)".
     Q numbers are identical across languages. -->

# <Title>

**In one sentence:** <the core answer, spelled out>

**Abstract:** <≤ 3 sentences: who asks what here, and what is the answer at its core?>

## 1. Problem

<Which question the compendium answers, who asks it, and in whose language — as a
situation, not a topic announcement.>

## 2. Overview

<The result first: the one table or the core statements, compact. Whoever has only one
minute reads only this section — and must not take away anything wrong.>

## 3. Terms

<One line per term, including the seemingly obvious ones. If empty: "omitted, because
the compendium works with common language.">

## 4. Details

<!-- One uniform block per subject: same fields, same order, so the document can be read
     crosswise ("what holds for X in field Y?"). The fields are fixed once per compendium
     and then kept — e.g. access · prerequisites · pitfalls · key data. A subject without
     data says "unknown" or "unverified", never simply nothing. -->

### <Subject 1>

**<Field A>.** <…>

**<Field B>.** <…>

### <Subject 2>

<!-- … same block … -->

## 5. Security and operational pitfalls

<How you notice in operation that something is not (or no longer) right: typical
failures, their symptom, their cause, the first move — the compendium version of the
symptom principle. Plus the security-relevant pitfalls where they exist; "omitted,
because …" only with justification.>

## 6. Scope and survey

<Survey period and method (what was verified first-hand, what was adopted from sources),
limits of validity — and the regeneration rule: "to be re-surveyed as soon as …" (a time
threshold or a named event, e.g. a vendor announcement). Facts age; the document itself
says from when on it is no longer to be trusted.>

## 7. State of research and open points

<Dated. What remained open, what was adopted unverified, where sources contradicted each
other — numbered Q1, Q2, …, stable across languages so discussion stays addressable.>

**Q1** <…>

## 8. Sources

<All references, only here. Primary sources (vendor/operator documentation) before
secondary ones; every source with its retrieval date.>
