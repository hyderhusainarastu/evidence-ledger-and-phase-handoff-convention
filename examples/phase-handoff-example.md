# Worked example — phase-to-phase handoff

> **SYNTHETIC EXAMPLE.** "Nightjar Reading Tools," all phase names, file
> paths, commit hashes, tags, and test counts below are fabricated for
> illustration only. No real project, codebase, deployment, or test suite
> is described here.

---

```markdown
# Phase 6 → Phase 7 handoff — Citation resolver hardening → Reader annotations

Status: Phase 6 complete and tagged `phase-6-complete`. Phase 7 remains
confirmation-gated pending the project owner's go-ahead.

## Decisions

- The citation resolver's coverage figure is corrected to 34/41 (82.9%),
  replacing the stale 28/35 figure carried in earlier planning documents —
  see `docs/evidence/T-04_resolver_coverage.md` for the verification pass.
- The 7 remaining fixture failures are triaged as Phase 7 candidates, not
  Phase 6 blockers: none of them affect the citation forms Phase 6's own
  acceptance scope (journal articles, books, book chapters) actually
  covers.

## Changed areas

- `packages/citations/src/rules/`: two rules (`journal-article.ts`,
  `book-chapter.ts`) hardened against malformed page-range input; no schema
  change.
- `packages/citations/test/fixtures/`: 6 new fixtures added covering the
  malformed-input cases above.
- `docs/status/citation-resolver.md`: coverage figure corrected to 34/41.

## Verification

- `pnpm --filter citations test:fixtures` — 34 passed, 7 failed (documented
  above as Phase 7 candidates, not Phase 6 regressions).
- `pnpm --filter citations typecheck` — clean.
- `pnpm -r lint` — clean.
- Full integration suite against a local database — **not run this pass**;
  Phase 6's scope never touched anything database-facing, so this is
  recorded as a scope note, not a deferred check.

## Remaining risks and gates

- The 7 failing fixtures need root-causing before Phase 7 can safely build
  citation-dependent reader annotations on top of the resolver — a false
  resolution surfaced as a correct one would propagate into annotation
  data. This is Phase 7's first task, not yet started.
- No production deployment has occurred this phase; Phase 6's changes exist
  only in the local/CI environment. Deploying is out of this session's
  authorization.

## Phase 7 prompt

Read this handoff, then `docs/PROJECT-LOG.md` (current state), then
`docs/status/citation-resolver.md` (the corrected coverage figure), then
`docs/evidence/T-04_resolver_coverage.md` (the verification pass this
handoff's Decisions section relies on). Begin only Phase 7's scope: reader
annotations that consume resolved citations. Preserve the resolver's
34/41-passing state — do not modify `packages/citations/src/rules/` as a
side effect of annotation work without its own dedicated verification pass.
Do not rely on any prior context beyond this handoff and the three files
named above.
```
