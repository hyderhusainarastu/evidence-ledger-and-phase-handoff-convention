---
name: evidence-ledger-and-phase-handoff-convention
description: Produces evidence ledgers (what was read, skipped, verified, unresolved) and phase-handoff briefs that let a new session resume with no other context. Use when finishing work others will rely on, or closing a phase or session.
---

# Evidence ledgers and phase-handoff conventions

## What problem this solves

In any workflow where multiple agents or multiple sessions build on each
other's work — a multi-agent pipeline, a long project worked in phases, a
review chain — two failure modes recur constantly:

1. **Unverifiable claims propagate.** Session B trusts a fact Session A wrote
   down, without knowing whether A actually opened the source that supports
   it, skimmed a summary of it, or is repeating something an even earlier
   session asserted. Errors compound silently across the chain.
2. **Context loss forces expensive re-derivation, or worse, confident
   guessing.** A new session either re-reads everything from scratch (slow,
   and it may not know what to re-read) or picks up where it thinks the prior
   session left off (fast, and often wrong).

This skill gives you two artifacts that close both gaps:

- An **evidence ledger**: a fixed-field, machine-scannable record attached to
  a piece of work, stating exactly what was consulted, what was deliberately
  not consulted, what status each claim carries, what's unresolved, and what
  a future reader needs to know before trusting any of it.
- A **phase handoff**: a short, self-contained document written at the close
  of a phase, workstream, or session that lets the next agent resume with
  *only this file* — no assumption that any other context survives.

Both are lightweight prose/Markdown conventions. Neither requires special
tooling — only discipline about *when* you write them and *what* goes in each
field.

## When to use each artifact

| You just finished... | Write |
|---|---|
| An analysis, audit, verification pass, research task, or any deliverable whose factual claims another session/reader will rely on | An **evidence ledger**, attached to or embedded in that deliverable |
| A phase, milestone, workstream, or long session, and work continues in a different session/agent/context next | A **phase handoff** |
| Both at once (the end of a phase that also produced claims someone will trust) | Both — the handoff can be short and simply point at the ledger-bearing artifacts it closes out |

Use an evidence ledger even for work you are confident in. Its value is not
hedging — it is making "what was actually checked" a first-class, reusable
fact instead of something buried in prose or lost when the session ends.

Use a phase handoff even when you expect to be the one who continues the
work. Sessions get interrupted, context windows fill, and "I'll remember"
is not a resumption strategy another agent (or your future self) can act on.

---

## Part 1 — The evidence ledger

### Core discipline (read this before filling in fields)

1. **Cite only what you personally opened this pass.** An evidence ledger
   entry names a file *this agent actually read during this pass*, with
   enough of a locator (line range, or "full file") that a future reader can
   re-open the exact same evidence. Never list a file because an earlier
   draft, a summary, or another agent's report claims to have opened it —
   if you didn't open it yourself, it belongs in `files_not_accessed`, or
   you cite the artifact that *did* open it as your source instead of the
   original.
2. **Every claim gets a status from a closed, small vocabulary — not a
   sentence of hedging.** A closed vocabulary is scannable and comparable
   across ledgers; free-text confidence language is not. See the status
   vocabulary below.
3. **Separate "a source asserts X" from "X is independently verified."** A
   prior document, a log file, or a teammate's report saying something is
   true is evidence that *a claim exists*, not evidence that it is *correct*.
   Record both: what is claimed, and how firmly this pass can stand behind
   it having checked it directly.
4. **Never let an outcome/effect claim borrow the status earned by a
   mechanism claim.** "The system has a feature that could produce effect X"
   and "effect X has been observed/measured" are different claims requiring
   different evidence. If your evidence proves the mechanism exists but not
   that the effect occurred, keep the effect claim's status separately
   marked `unsupported_open` (or equivalent) even if the mechanism claim is
   `verified`. This single discipline prevents the most common way ledgers
   quietly overstate what's known.
5. **Record what you deliberately did *not* check, not just what you did.**
   `files_not_accessed` is not boilerplate. List anything a reasonable
   reader might assume you checked (an adjacent module, an external
   database, a sibling repository) and say why it's out of scope — silence
   here reads as "not relevant," which is a claim in itself.
6. **Conflicts and unknowns are first-class fields, not asides in prose.**
   When two sources disagree, or a fact simply can't be determined from
   what's available, put it in `conflicts_or_unknowns` with whatever
   governing rule breaks the tie (e.g., "the more current source wins,"
   "the primary artifact outranks a summary of it") or, if nothing resolves
   it, mark it explicitly open and name who owns resolving it.
7. **A ledger is append-only once anything else may depend on it.** If a
   later pass finds an error in an existing ledger, don't silently rewrite
   the original text. Add a dated, clearly marked correction block (see
   "Repairing an existing ledger" below) that states what changed and why,
   leaving the original visible above the marker. Anything that already
   cited the old wording needs to still be able to find it.
8. **Don't edit an artifact you don't own.** If a ledger's fix belongs in a
   *different* file or a different task's deliverable, don't reach over and
   patch it yourself — record the routing (what needs to change, and where)
   and let the owning task or a differently-scoped session make the edit.
9. **Give non-prose outputs a sidecar ledger.** A CSV, JSON, or other
   non-Markdown data file can't hold ledger prose comfortably. Create a
   Markdown file with the *same basename* (`results.csv` →
   `results.md`) whose entire content is that file's evidence ledger.
10. **A blocking finding is closed by a fresh check, not by the author of
    the fix.** If this convention feeds into any kind of review or audit
    process, whoever wrote or repaired an artifact should not also be the
    one who declares their own fix verified. Route closure to a different
    reviewing pass explicitly, and say so in `handoff_notes`.

### Status vocabulary

Use a small, closed set of values for every claim. Six values cover nearly
everything in practice — adapt the exact wording to your project, but keep
it closed and keep the same distinctions:

| Value | Meaning |
|---|---|
| `verified` | Directly confirmed against primary evidence during this pass |
| `asserted_unverified` | A source states this, but this pass did not independently confirm it |
| `partial` | True only in part, only behind a flag/condition, or only for a subset of scope |
| `planned` | Not yet true; stated as intended future work by a source |
| `retired` | Was true or claimed previously; explicitly withdrawn or superseded |
| `unsupported_open` | No evidence found either way; not asserted as fact in either direction |

### The template

Place this as a section in the deliverable it documents (commonly titled
`## Evidence Ledger`, `## §9 Evidence Ledger`, or similar if your project
numbers a governing convention document), or as the entire content of a
sidecar file for non-Markdown outputs. The full field-by-field template with
inline guidance lives in `references/evidence-ledger-template.md`; the
compact version:

```text
agent_model_identity: <who/what did this pass — model or agent identifier,
  role, task/phase id>. <One sentence on any boundary this identity
  respected, if relevant — e.g. "authored no judgment on X, only Y">.

files_consulted:
  - <path>
    <lines/sections read, or "full file">
    claim(s) supported: <what this file backs, and which claims below>
  - <path>
    ...

files_not_accessed:
  - <path or category>
    reason: <why out of scope, or what a reader might wrongly assume was checked>

project_claims:
  - claim: "<precise statement of the claim>"
    status: <one value from the status vocabulary>
    evidence: <locator: file:line-range, or "none">
  - claim: "<...>"
    status: <...>
    evidence: <...>

external_sources: <"none — reason", or list of {source, access date, what it supports}>

conflicts_or_unknowns:
  - <description of the conflict or unknown>
    governing rule / resolution: <which source wins and why, or "OPEN — owner: <who>">

sections_affected: <downstream documents/sections/artifacts this work
  should update, or that depend on these claims>

handoff_notes: |
  <what's stale and needs re-checking before being trusted further; what NOT
  to treat as settled; what to read alongside this file; anything a future
  reviewer should verify independently rather than inherit from this pass>
```

### Repairing an existing ledger

When you must correct a ledger another artifact may already cite:

1. Never delete or rewrite the original entry text.
2. Add a clearly dated marker before your addition, e.g.:
   `**Repair note (<date>, <your identity>):** ...` or a fenced
   `--- REPAIR PASS (<date>) ---` divider inside a ledger's code block.
3. State plainly what was wrong, what you changed, and why — and update the
   corrected file's own `files_consulted` / `project_claims` /
   `conflicts_or_unknowns` to reflect the correction, not just the prose
   above it. A correction that only lives in prose and never reaches the
   structured fields leaves the ledger's own machine-scannable record stale.
4. If the correction doesn't fully resolve an open, blocking question, say
   so explicitly rather than implying the repair closes it — a repair and a
   *verified* resolution are different claims (see discipline #10 above).

---

## Part 2 — The phase handoff

### Core discipline

- **Write for zero inherited context.** Assume the next reader has this file
  and nothing else from your session — not your reasoning, not your tool
  output, not the rest of the conversation. Everything they need to safely
  continue must be *in* the handoff or reachable by an exact reference it
  gives them.
- **State the real status, including an honest "not actually done" when
  that's true.** If a mechanism you'd normally rely on to hand off cleanly
  (a context reset, an approval, a deploy) isn't available, say so plainly —
  "this is an intentional stop, not a completed handoff" — rather than
  writing the document as if the clean handoff happened. Never let the act
  of writing the handoff imply a state transition that didn't actually occur.
- **Name the next reader's exact reading order.** Don't just say "read the
  project docs" — list the specific files, in the order they should be
  opened, so context is rebuilt deterministically rather than however the
  next agent happens to explore.
- **Separate what's authorized from what isn't.** If any part of the next
  phase's scope needs approval this session didn't have (a production
  change, a paid external call, an irreversible action), say so as its own
  labeled boundary — don't bury it in a risks list where it can be missed.
- **Distinguish "remaining risks" from "explicit limitations."** A risk is
  something that could go wrong later; a limitation is something you already
  know you didn't do or couldn't fully verify. Conflating them makes both
  harder to act on.
- **Give the next phase a concrete start condition**, not just a topic — the
  exact trigger that means it's safe to begin (e.g., an explicit go-ahead
  from whoever owns that decision, or a specific prior step's completion).

### Template — phase-gate handoff

Use this shape when a phase in a sequential program completes and the next
phase is gated behind confirmation. Full template with guidance:
`references/phase-handoff-template.md`.

```markdown
# <Phase name/number> handoff — <one-line description of what this phase covers>

Status: <complete and tagged `<tag>`; <next phase> remains
confirmation-gated> | <blocked — state exactly why, and that this is an
intentional stop, not a claimed completion>

- <what changed structurally — schema/infrastructure, additive vs. breaking,
  where it's applied (local/staging/production, or their equivalents)>
- <the core behavioral or functional change this phase delivered, in plain
  terms a non-implementer can follow>
- <an invariant this phase preserved, or a boundary it must not cross —
  stated so the next phase can't accidentally violate it>
- <verification performed: name the actual checks/suites run and their
  results (counts, pass/fail), plus any deployment/version identifiers>

Next: <the exact condition required before starting the next phase>. <any
standing prohibition that carries forward — e.g., "do not touch or commit
<path>.">
```

### Template — detailed phase-to-phase handoff

Use this fuller shape for higher-stakes transitions, or when the phase
gate blocks on something outside the agent's own authority (e.g. no
mechanism exists to hand off context cleanly, or approval is still
pending).

```markdown
# <Phase N> → <Phase N+1> handoff

Status: <local acceptance complete | complete | blocked — and *why*, stated
plainly if this is a fail-closed stop rather than a claimed completion>

## Decisions
- <decision, stated as fact, with enough rationale to not need re-litigating>
- ...

## Changed areas
- `<path/module>`: <one line on what changed here>
- ...

## Verification
- `<exact command or check run>` — <N passed / M failed>
- ...
- <anything explicitly left "pending final rerun" rather than silently
  dropped — a documented gap is honest; a silent one isn't>

## Remaining risks and gates
- <what's outstanding and what authorization/condition it needs before it
  can proceed>
- <a known limitation, stated at its real severity, not minimized>

## <Next phase> prompt
Read this handoff, then <ordered list of the exact canonical files to read
next>. Begin only <next phase>'s scope. Preserve <the specific invariants
or boundaries this phase established that must not regress>. Do not rely on
any prior context beyond this handoff.
```

### Template — cross-workstream contract handoff

Use this variant when one workstream is handing a technical contract (a
schema, an interface, a data shape) to a *different*, not-yet-started
workstream, rather than handing off a whole phase.

```markdown
# <Workstream> handoff — <topic>

## <What was actually wrong, and the fix>
<enough technical detail — root cause, not just symptom — that a fresh
reader can verify the fix independently rather than take it on trust>

## <Contract for the receiving workstream>
<the schema/interface/data shape being handed over>

Required fields/columns: <table: name | required shape>

Required constraints/behavior:
- <constraint>
- **Decision required from the receiving owner:** <anything this handoff
  deliberately leaves open rather than silently deciding on the receiving
  team's behalf — flag it, don't resolve it for them>

The receiving workstream then wires this in without changing the handed-off
module itself:
1. <integration point 1 — what reads from the contract>
2. <integration point 2 — what writes to it>

<Any hard rule the integration layer must respect — ordering, batching,
things it must never do.>
```

### Recommended resumption reading order

Pair the phase handoff with a fixed, documented order for how *any* new
session should rebuild context, not just the one immediately following this
phase:

1. The handoff file itself (most recent, most specific).
2. The project's canonical persistent-memory document (a single file that
   reflects actual current state — not the plan's aspirational state — and
   is kept current after every meaningful step).
3. Any structured status/tracker file (e.g. a machine-readable phase/task
   status record), if the project maintains one.
4. Version-control history (recent log, most recent tags/checkpoints) to
   confirm nothing landed after the handoff was written.

Publishing this order once, in the canonical memory document itself, means
every future handoff can simply say "read this handoff, then follow the
project's standard resumption order" instead of re-deriving it each time.

---

## Self-check before finalizing either artifact

Evidence ledger:
- [ ] Every `files_consulted` entry names a file this pass actually opened, with a real locator.
- [ ] Every `project_claims` entry has a status from the closed vocabulary — no free-text hedging standing in for a status.
- [ ] No claim is on record without either an evidence locator or an explicit `unsupported_open`/`asserted_unverified` marking.
- [ ] No effect/outcome claim has inherited a `verified` status earned only by a mechanism claim.
- [ ] `files_not_accessed` lists anything a reader might otherwise assume was checked.
- [ ] `conflicts_or_unknowns` covers every place two sources disagreed, or a fact stayed unknown — none of that is quietly resolved in prose only.
- [ ] If this is a correction to an existing ledger, the original text is intact and the correction is dated and visibly marked.

Phase handoff:
- [ ] A reader with only this file (no other context) has everything needed to safely continue.
- [ ] The status line states the real situation, including an honest "not actually complete" if that's true.
- [ ] The next reader's reading order is an explicit, ordered list of exact files — not a general pointer.
- [ ] Anything needing approval this session didn't have is called out as its own boundary, not folded into a general risk list.
- [ ] The condition for starting the next phase is concrete and checkable, not just a topic name.
- [ ] Nothing in the document implies a clean state transition (context replaced, phase closed, deploy live) that didn't actually happen.

---

## Files in this skill

- `references/evidence-ledger-template.md` — full field-by-field template with inline guidance and a worked note on the status vocabulary.
- `references/phase-handoff-template.md` — full templates for all three handoff shapes (phase-gate, detailed phase-to-phase, cross-workstream contract) with inline guidance.
- `examples/evidence-ledger-example.md` — a complete, synthetic worked example (fabricated project, fabricated file paths and figures, clearly labeled).
- `examples/phase-handoff-example.md` — a complete, synthetic worked example of a phase-to-phase handoff (same fabricated project).
