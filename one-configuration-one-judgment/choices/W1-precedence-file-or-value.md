---
type: Proposal Choice
description: Whether precedence selects which file is read, with all values coming from that file and the command line only choosing the file, or applies to each value individually across command line, environment and files.
number: W1
granularity: per-setting
applies_to: [builder, operator]
---

# W1 · Precedence: file or value

**The choice.** Whether the precedence determines *which file* applies,
or whether it applies *per value*. Decided per setting, in practice per
group of settings, by the builder together with the user.

**(a) File precedence.** Command line, environment, user-level place,
system-level place determine which file is read; all values of the group
come from that one file. The command line and the environment only
select the file (`--config PATH`, `SERVICE_CONFIG=PATH`); they do not
override single values. If no file is found, the check says so and
reports every value as a default. Consequences: the duty to report
([F2](../rules/F2-the-check-names-the-source.md)) reads "which file";
[F5](../rules/F5-duplicate-setting-is-reported.md) does not apply. The
configuration can be versioned, checked and rolled back as a whole. Fits
where the configuration is maintained as a whole and the platform can
inject files.

**(b) Per-value precedence.** The same order applies to each value: a
value from the file can be overridden by the command line or the
environment. Consequences: F2 per value, F5 applies; the builder defines
and documents an environment name per value. Fits where the platform
sets values individually, containers, platform-set variables, and a file
for a single value would be disproportionate.

**Rejected for all options.** *Everything only through environment
variables*: no check of the whole, no versioning, no rollback.

**Record your decision.** Per group of settings, with the reason;
deviations of individual settings individually. If in doubt, (a) for a
service that is operated by hand, (b) for one that is operated by a
platform.
