# Worked example — evidence ledger

> **SYNTHETIC EXAMPLE.** "Nightjar Reading Tools," all file paths, task ids,
> counts, and findings below are fabricated for illustration only. No real
> project, codebase, or dataset is described here.

---

# T-04: Citation-resolver coverage check — sidecar

**Task:** T-04 (verification lane), scope: confirm whether the citation
resolver's stated 80%-coverage claim holds against the current codebase in
`your-project/packages/citations/`.

## 1. Method

1. Read `docs/status/citation-resolver.md` (the document making the 80%
   claim) in full.
2. Re-ran the resolver's own fixture suite locally and recorded pass/fail
   counts directly, rather than trusting the status document's summary.
3. Spot-checked five resolver source rules against the fixture inputs they
   claim to cover.

## 2. Findings summary

The fixture suite currently passes 34 of 41 cases (82.9%), not the 80% the
status document states — close, but the status document's own cited count
(28 of 35) no longer matches the fixture directory, which has grown since
that document was last updated.

---

## Evidence Ledger

```text
agent_model_identity: verification-worker-b (synthetic), task T-04. No
  judgment recorded here about whether 80% coverage is an adequate target —
  only whether the coverage figure is currently accurate.

files_consulted:
  - your-project/docs/status/citation-resolver.md
    full file
    claim(s) supported: the existing "28/35, 80%" claim this task checks

  - your-project/packages/citations/test/fixtures/
    directory listing — 41 fixture files (was 35 at the status document's
    last revision date, per its own header)
    claim(s) supported: the fixture count discrepancy noted in
    conflicts_or_unknowns below

  - your-project/packages/citations/src/rules/journal-article.ts
    lines 12-48 (full rule body)
    claim(s) supported: spot-check that this rule's fixtures still pass
    under current logic

  - your-project/packages/citations/src/rules/book-chapter.ts
    lines 5-30
    claim(s) supported: same spot-check, second rule

  - test run output (this pass): `pnpm --filter citations test:fixtures`
    34 passed, 7 failed, run directly by this pass
    claim(s) supported: the corrected 34/41 figure

files_not_accessed:
  - your-project/packages/citations/src/rules/ (the remaining 9 rule files
    beyond the two spot-checked above) — reason: task scope is coverage
    accounting, not a full rule-by-rule correctness audit; the two
    spot-checked rules were chosen as the largest fixture-count owners, not
    because the others are assumed fine.
  - your-project/apps/web/ — reason: this task checks resolver-package
    coverage only, not any UI that consumes resolver output.

project_claims:
  - claim: "The citation resolver's fixture suite currently passes 34 of 41
    cases (82.9%)."
    status: verified
    evidence: test run output, this pass — see files_consulted above

  - claim: "The status document's cited '28/35, 80%' figure is stale, not
    wrong at the time it was written."
    status: verified
    evidence: fixture directory now holds 41 files vs. the 35 the status
    document's own header date implies; no evidence the original 28/35
    figure was inaccurate when recorded

  - claim: "The resolver correctly handles all book-chapter citation forms."
    status: unsupported_open
    evidence: none — only the fixtures currently in the suite were checked;
    this task makes no claim about citation forms the fixture suite doesn't
    cover

sections_affected: your-project/docs/status/citation-resolver.md (its
  headline figure needs updating to 34/41); any downstream planning
  document that cites the 80% figure as current.

conflicts_or_unknowns:
  - conflict: status document states "28/35, 80%"; this pass's direct test
    run states "34/41, 82.9%."
    governing rule / resolution: a freshly run test result outranks a
    prior narrative summary of a test result. 34/41 is the current, correct
    figure; 28/35 was accurate only as of the status document's own
    revision date, which predates 6 fixtures added since.
  - unknown: whether the 7 currently-failing fixtures represent real
    resolver defects or fixtures that were added expecting behavior not yet
    implemented.
    governing rule / resolution: OPEN — owner: whichever task next touches
    the resolver's rule files; this task's scope was coverage accounting,
    not root-causing individual failures.

external_sources: none — task scope is local repository verification only.

handoff_notes: |
  Update docs/status/citation-resolver.md's headline figure to 34/41
  (82.9%) before any planning document cites the old 80%/28-35 figure
  again. The 7 failing fixtures are listed in this pass's raw test output
  (not reproduced here) and are a reasonable next task on their own — do
  not assume they're all real defects without opening each one; some may
  be fixtures written ahead of the rule that should satisfy them.

  This ledger makes no claim about resolver correctness beyond the fixture
  suite's own coverage. A future pass wanting to claim broader correctness
  needs its own evidence, not an inheritance of this one's "verified"
  status.
```
