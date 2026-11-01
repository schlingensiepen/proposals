---
type: Proposal Agent Guide
name: one-operation-many-surfaces
description: Self-contained guide for an agent that builds or reviews a service with more than one surface (a command line, an HTTP surface, further ones) so that every administrative action exists exactly once and every surface is a pass-through onto it, held by a structural test; procedure, rules, acceptance checklist, common mistakes, self-check.
---

# Agent guide

## Purpose

Apply this guide when you build or review a service that can be reached over more than one surface, a command line and beside it a second surface such as an HTTP surface, or further ones, and you have to keep each administrative action from being implemented once per surface. Your role is the builder. This guide is complete on its own; this concept has no choices to decide. Rules are named in parentheses as provenance.

## Procedure

**1 · Secure the operations.** Make sure every administrative action exists as a callable function apart from any command and any handler, raising its own error kinds. If the logic lives in the commands today, extract it first. (F1)

**2 · Write the structural tests before the first handler.** Whitelist of imports, list of operation names, file system ban with named exceptions, "every command and handler calls an operation". Run each once with its protection removed. (F2)

**3 · Build the routes as pass-throughs.** Mirror every command. (F1)

## Rules

MUST:

1. Implement every administrative action **exactly once**, as a callable function that knows neither HTTP nor process exit: it takes arguments, acts, and returns a result or an error kind of its own vocabulary. Make the command line and the second surface **pass-throughs** onto those functions: each command and each handler reads its arguments, calls one operation, and returns what came back. Give each surface exactly one translation function (error kinds to HTTP statuses on one side, to exit codes on the other) and let both emit the same response models with the same field names. No surface writes the store, touches the file system or talks to the target system; the operations do. A surface that needs something no operation offers gets a new operation, not a search of its own. (F1)
2. Hold that property with a **structural test** in the build that parses the source of both pass-through packages and fails where an operation is performed instead of called. Check at least: that a pass-through imports from the service only the operations and the operational parameters (a **whitelist**, never a blacklist); that the names of the operations themselves (read or write the store, generate or verify a token, enqueue, claim, revive or discard a job, store, list or delete a file, the client for the target system) appear in no pass-through; that no pass-through opens, writes, lists or removes anything on the file system, with any exception named in the test by function and file rather than matched by pattern; and that every command and every handler calls at least one operation, and everything it calls really is one. Run each of these assertions once with its protection removed and see it fail before you trust it. (F2)

NEVER:

- Never give one surface a capability the other lacks, and never implement an operation in a surface instead of calling it. (F1)
- Never rely on behavioral tests alone to keep the two surfaces on one operation; two correct copies pass every behavioral test. (F2)

## Acceptance checklist

- [ ] Does every administrative action exist exactly once as a function without HTTP or exit knowledge, do both surfaces call it, do both have exactly one translation function, and do both emit the same field names? (F1)
- [ ] Does a structural test over both pass-through packages fail on a forbidden import, an operation name, a file system call, or a command or handler that calls no operation, and has each assertion been seen red once? (F2)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| A helper in a command "just for output" that reads the store | Make it an operation; the structural test must go red on the import (F1, F2) |
| Structural test written as a blacklist | Whitelist of allowed imports; a blacklist passes every module nobody thought of (F2) |
| File system exception matched by pattern | Name it by file and function; a narrower pattern admits the next reader (F2) |

## Self-check

Before you report completion: can you name, for each administrative action, the one operation behind it; the one translation function of each surface; the structural test that holds the oneness, with its whitelist, its list of operation names and every named file system exception; and, for each assertion, that you saw it red with its protection removed? If any of these is missing, you are not done.
