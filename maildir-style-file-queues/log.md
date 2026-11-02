# Update log

## 2026-11-01
* **Update**: Version 1.0. Retention of the journal and the disk is a choice: `choices/W1-retention.md` with four options (nothing automatic; a retention period for the journal; a size quota; both), uploads excluded in every option, (a) the fallback without a decision. F8 names W1 for the journal and the quota; the statement that uploads are never deleted automatically is unchanged, and the statement that only the staging folder is cleared automatically now holds beyond what W1 allows. The scope moves quotas and the retention period out of "not yet settled" and names the price of each option; the expectation of a mounted volume with ordinary rename semantics and the condition that one service process owns the tree are now preconditions. The agent guide has the step of deciding the choice with the user; the index and README have the group Choices. Wording: field title "Considered and rejected", section "When this no longer holds", calques smoothed; content of the other rules unchanged.
* **Update**: Form (agent guide name, README describes the parts, relation vocabulary).

## 2026-10-13
* **Update**: Bundle form. The material version 0.9 translated into English and split into files: concept, terms, nine rules (F1 to F9, numbers unchanged), scope, evidence, sources, agent guide. The open questions of 0.9 became duties of adoption in the agent guide (retry schedule and final failure, the threshold at which the queue becomes unwieldy) or "not yet settled" in the scope (quotas and eviction, journal retention, backup, file systems with different rename semantics). Content unchanged; version stays 0.9.

## 2026-08-15
* **Creation**: Version 0.9, material version. Taken from the design work on the author's draft-filing service: everything decided and considered there on this topic, including the rejected paths. Nine rules, six open questions, complete in content; not yet reviewed by the author, in German only.
