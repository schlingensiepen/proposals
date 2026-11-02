# Update log

## 2026-11-01
* **Update**: Version 1.3, wording only; content of the rules unchanged.
* **Update**: The agent guide names exactly what it takes from each imported concept (`one-configuration-one-judgment`, `where-a-secret-comes-from`, `maildir-style-file-queues` for the atomic replacement only, `build-stamp`) and links choice W1 in step 6 instead of listing the choice file as an exception.
* **Update**: Field title "Obvious and rejected" renamed "Considered and rejected" in all thirteen rules.
* **Update**: Section "When the answer flips" renamed "When this no longer holds" in the scope; index and README follow.
* **Update**: German calques replaced in all files ("whoever …" → "if you …"/"anyone who …", "lies/sits/stands in" → "is in", "foreign" → "external"/"someone else's", "the cut" → "the design"/"the separation"); "permission cut" kept as the name of F8 and added to the terms.
* **Update**: Evidence: a stale reference to question Q3 and to "Source 2" replaced by references to the scope and the sources.
* **Update**: F12 names the version marker and the lock file as its own requirements; only the atomic replacement is imported from `maildir-style-file-queues` (review before release).
* **Update**: Version 1.2. F4 gains one sentence: the deliverable says which revision it was built from, and the build pipeline publishes nothing that cannot; rule and reasons are imported from `build-stamp`, now listed in the imports. Steps 6 and 9 of the procedure, the rules summary and the acceptance checklist of the agent guide follow. Form: the agent guide carries `name` for skill loaders; the README describes and links the parts and says where readers and agents start; related proposals in the sources carry their relation (`recommended-with`, `overlaps-with`); `one-operation-many-surfaces` added as related.

## 2026-10-11
* **Update**: Context-free reading test before publication: procedure, rules and scope brought into line with F12 and F13 (variants are store, not configuration; F6 and F8 reworded; F9 states what the change command may change and when a hash is salted; F13 records the structure version from the first start); section and question references of 1.0 replaced; evidence marked as describing the 1.0 reference; wording smoothed.
* **Update**: Version 1.1. Two new rules: F12 configuration versus state (the variants are the store, state created by the artifact itself; concurrency mechanics imported from `maildir-style-file-queues`) and F13 data migration at start. F5 reduced to "all modes read with the same code, the check is a mode"; precedence, the empty value and the origin of start secrets moved into the imported concepts `one-configuration-one-judgment` and `where-a-secret-comes-from`. F2 states the form of the artifact (binary, package or directory); F4 treats the container case as an example; F6 and F8 use the terms of F12; F9 persists the secrets of a variant in the store; F11 justified by ready-made proxy containers. New choice W1, the channel of the commands. The term "command" sharpened. The open questions of 1.0 became duties of adoption, F13, or "not yet settled" in scope. First version in bundle form.

## 2026-08-15
* **Creation**: Version 1.0, first complete version. A procedure in eleven steps and eleven rules: cut (F1 to F6), separation (F7 to F10), outer edge (F11). Checked twice in practice before publication by a context-free agent building another tool from the dossier and the imap-sync request alone; four corrections from the first run, self-containment confirmed by the second.
