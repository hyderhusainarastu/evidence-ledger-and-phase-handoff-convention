# Evidence ledger — full template and field guidance

Copy the fenced block below and fill in every field. Do not drop a field
because it feels empty for this task — write `none` (or `none — reason`)
explicitly. An absent field reads as "forgotten"; an explicit `none` reads
as "considered and ruled out."

```text
agent_model_identity: <identifier>

files_consulted:
  - <path>
    <locator>
    claim(s) supported: <...>

files_not_accessed:
  - <path or category>
    reason: <...>

project_claims:
  - claim: "<...>"
    status: <verified | asserted_unverified | partial | planned | retired | unsupported_open>
    evidence: <...>

external_sources: <...>

conflicts_or_unknowns:
  - <...>
    governing rule / resolution: <...>

sections_affected: <...>

handoff_notes: |
  <...>
```

## Field-by-field guidance

### `agent_model_identity`

Name whoever/whatever produced this pass: a model identifier, a role name,
a task or phase id — whatever your project uses to distinguish one working
session from another. If a governance rule constrains what this identity
may or may not do (e.g., "this role may verify but not author," "this pass
may not close its own prior finding"), say so in one sentence here so a
reader doesn't have to reconstruct the boundary from context.

If this is a **repair pass** on an existing ledger, name both identities:
the original author (unchanged) and the repair identity, and make clear
which content belongs to which — see "Repairing an existing ledger" in
SKILL.md.

### `files_consulted`

One entry per file *actually opened during this pass*. For each:

- The path, exactly as referenced elsewhere in the project.
- A locator: a line range, a named section, or `full file`. A locator that
  says "the whole file, generally" is weaker evidence than one that says
  "lines 40–55, the `X` function" — be as specific as the claim needs.
- `claim(s) supported`: which claim(s) below this file backs. This is what
  turns a reading list into an actual evidence trail — without it, a reader
  can't tell which file backs which fact.

Never list a file you learned about secondhand (another agent's report, a
prior draft, a summary document) as something *you* consulted. If you only
have secondhand knowledge of a file's content, either open it yourself
before citing it, or cite the *secondary* source you actually read and
say plainly that the underlying primary file was not independently opened
this pass.

### `files_not_accessed`

List anything a reasonable reader might assume was in scope, and say why it
wasn't opened. Good reasons: "out of this task's stated scope," "a different
task's evidence already covers this and is cited there," "read-only
reference material not relevant to this claim set." A bad non-reason is
silence — an omitted file reads as "not relevant," which is itself an
unstated claim.

If your task is deliberately scoped away from an entire directory or
repository (a common pattern when several agents divide a codebase), name
that boundary explicitly here even if it feels obvious from the task
description — a future reader of just this ledger may not have the task
description in front of them.

### `project_claims`

One entry per substantive claim this pass makes or evaluates. Keep the
`claim` field a precise, quotable statement — not a summary of several
things at once. If a claim has two parts (e.g. "X is built" and "X works as
intended for users"), split it into two `project_claims` entries so each
gets its own status; do not let a compound claim borrow the stronger half's
status for the weaker half.

`status` must be one of the closed vocabulary values (see SKILL.md). Do not
substitute prose like "mostly true" or "probably fine" — if the true state
is nuanced, that's what `partial` plus a note in `evidence` is for.

`evidence` is a locator into `files_consulted` (or `external_sources`) — not
a restatement of the claim. If there genuinely is no evidence either way,
write `none` and make sure the status is `unsupported_open`, not something
stronger.

### `external_sources`

If this pass used anything outside the project's own files (a web search, a
vendor's documentation, a published reference), list each as
`{source, access date, what it supports}`. If none were used, write
`none — <why>` (e.g., "task scope is read-only over local files only," or
"no external fact was needed to support any claim above"). Do not write
bare `none` with no reason — the reason is what lets a reader trust the
absence was deliberate, not an oversight.

### `conflicts_or_unknowns`

Every place two sources disagreed, or a fact could not be determined from
what was available. For each:

- Describe the conflict/unknown concretely enough that a reader doesn't
  have to reopen every cited file to understand it.
- State the governing rule that resolves it, if one exists in your project
  (e.g., "the more recently dated source wins," "the primary artifact
  outranks any summary of it," "code state outranks a narrative status
  claim about the code"). Apply the rule and say what it resolves to.
- If no rule resolves it, mark it explicitly `OPEN` and name an owner —
  whoever is positioned to resolve it next. An unresolved conflict with no
  owner tends to stay unresolved indefinitely.

Do not let a conflict get quietly absorbed into ordinary prose elsewhere in
the deliverable — if it's worth mentioning once, it's worth a line here so
a reader scanning only this field catches it.

### `sections_affected`

Name the downstream documents, sections, or artifacts that either (a)
should be updated as a result of this work, or (b) depend on these claims
being correct going forward. This is what lets a later change propagate —
without it, a fact that changes later has no list of what else needs to be
revisited.

### `handoff_notes`

Freeform, but purposeful — this is where you write directly to whoever
reads this ledger next:

- What's already stale by the time they're reading it, and needs a fresh
  check rather than being trusted as-is.
- What NOT to treat as settled, even though it's written down here.
- What else to read alongside this file for full context.
- Anything you'd want a fresh, independent reviewer to re-verify rather than
  inherit from this pass unchallenged.

A ledger with an empty or perfunctory `handoff_notes` field has usually
skipped its most useful part.

## Adapting the status vocabulary

The six-value vocabulary in SKILL.md is a good default, but the only real
requirements are:

1. It's **closed** — a fixed, named set, not open-ended prose.
2. It **separates "claimed" from "verified."**
3. It has a value for **"neither confirmed nor denied."**
4. It has a value for **"was true, no longer is."**

If your project already has its own status vocabulary (a QA pipeline's
pass/fail/blocked states, a research pipeline's confidence tiers), reuse it
here rather than introducing a second, competing vocabulary — consistency
across a project's artifacts matters more than matching this exact list.
