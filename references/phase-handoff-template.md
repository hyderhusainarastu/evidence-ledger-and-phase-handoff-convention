# Phase handoff — full templates and field guidance

Three shapes cover nearly every handoff need. Pick the lightest one that
fits; don't pad a simple handoff with sections it doesn't need.

## Shape 1 — Phase-gate handoff (lightest)

Use when a phase in a sequential program completes cleanly and the only
thing the next phase needs is a compact summary plus its start condition.

```markdown
# <Phase name/number> handoff — <one-line description of what this phase covers>

Status: <complete and tagged `<tag>`; <next phase> remains
confirmation-gated> | <blocked — state exactly why>

- <structural change: schema/infrastructure, additive vs. breaking, where applied>
- <the core behavioral/functional change this phase delivered, in plain terms>
- <an invariant preserved, or a boundary the next phase must not cross>
- <verification performed: actual checks/suites run and their results, plus
  any deployment/version identifiers>

Next: <exact condition required before starting the next phase>. <any
standing prohibition that carries forward>.
```

Keep this to roughly 4–6 bullets plus the status and next-condition lines.
If you find yourself needing more structure than that, move to Shape 2.

### Field guidance

- **Status line**: always name the completion tag/marker if your project
  uses one, and always state the next phase's gating condition in the same
  line — don't make a reader hunt for whether they may proceed.
- **Bullets**: order roughly infrastructure → behavior → invariant →
  verification. A reader skimming just the first bullet should still learn
  something concrete, not a vague "things went well."
- **Verification bullet must name real numbers or real checks** — "tests
  passed" is not verification; "the 40-check integration suite passed,
  12/12 accessibility checks clean, deployed at commit `<hash>`" is.
- **Next line**: the condition must be checkable by the next reader without
  needing to ask anyone. "Only after explicit confirmation" is fine *if*
  your project has a clear, known mechanism for what "confirmation" means
  and who gives it.

## Shape 2 — Detailed phase-to-phase handoff

Use for higher-stakes transitions, or when something about the handoff
itself is imperfect and needs to be said plainly (no clean context-reset
mechanism, approval still pending, partial completion).

```markdown
# <Phase N> → <Phase N+1> handoff

Status: <local acceptance complete | complete | blocked — and *why*, stated
plainly if this is a fail-closed stop rather than a claimed completion>

## Decisions
- <decision, stated as settled fact, with brief rationale — enough that
  the next phase doesn't re-litigate it from scratch>
- ...

## Changed areas
- `<path/module>`: <one line on what changed here>
- ...

## Verification
- `<exact command or check run>` — <N passed / M failed>
- ...
- <anything explicitly left "pending final rerun" — a documented gap beats
  a silently dropped one>

## Remaining risks and gates
- <what's outstanding, and what authorization/condition it needs>
- <a known limitation, stated at its real severity>

## <Next phase> prompt
Read this handoff, then <ordered list of exact canonical files to read
next>. Begin only <next phase>'s scope. Preserve <the specific invariants
or boundaries this phase established>. Do not rely on any prior context
beyond this handoff.
```

### Field guidance

- **Status line honesty.** If the mechanism that would make this a "clean"
  handoff (a context reset, an atomic session replacement, a sign-off)
  isn't actually available to you right now, say so explicitly — e.g. "this
  is an intentional fail-closed stop, not a claimed handoff." Writing the
  document is not the same as the transition having happened; never let the
  existence of the document imply otherwise.
- **Decisions** are for anything the next phase would otherwise be tempted
  to re-derive or re-argue. State it once, with rationale, so it's settled.
- **Changed areas** is a map, not a diff — one line per area is enough; it
  tells the next reader *where to look*, not everything that happened there.
- **Verification** should be copy-pasteable: the actual command or check
  name, and the actual result. If a check was skipped or deferred, list it
  as such rather than omitting it — an omitted check looks like it wasn't
  needed; a listed-and-deferred check looks like exactly what it is.
- **Remaining risks and gates** vs. **explicit limitations** (fold
  limitations into this section if your handoff doesn't need a separate
  one, but keep the distinction in the wording): a risk is something that
  *could* go wrong later; a limitation is something you already know is
  incomplete right now. Don't let a known limitation hide inside "risks"
  where it reads as merely possible rather than already true.
- **The next-phase prompt is the single most load-bearing paragraph in the
  document.** It must give: (1) an exact, ordered reading list, (2) an exact
  scope boundary for what to do next, (3) an exact list of what must not
  regress, (4) an explicit statement that no other context should be
  assumed. Missing any of the four is the most common reason a handoff
  fails in practice — the next agent either re-reads everything (slow) or
  assumes too much (wrong).

## Shape 3 — Cross-workstream contract handoff

Use when one workstream is handing a technical contract — a schema, an
interface, a data shape — to a *different*, often not-yet-started
workstream, rather than handing off a whole phase to the same lineage of
work.

```markdown
# <Workstream> handoff — <topic>

## <What was actually wrong, and the fix>
<root cause, not just symptom — enough technical detail that a fresh reader
can verify the fix independently>

## <Contract for the receiving workstream>
No <schema/table/interface> is included in <this workstream> itself.
<State plainly what does and doesn't already depend on the contract, so the
receiving workstream knows exactly what's safe to build against.>

<Receiving workstream> should supply:

| Field/column | Required shape |
|---|---|
| `<name>` | <type, constraints> |
| ... | ... |

Required constraints/behavior:
- <constraint>
- **Decision required from the receiving owner:** <anything this handoff
  deliberately leaves open rather than silently deciding for them>

<Receiving workstream> then wires this in without changing the handed-off
module itself:
1. `<file/module>` <reads from the contract and does X>
2. `<file/module>` <writes to the contract and does Y>

<Any hard rule the integration layer must respect — ordering, batching,
what it must never do.>
```

### Field guidance

- **Root-cause section** exists so the receiving workstream doesn't have to
  take the fix on faith — give them enough to independently confirm it
  themselves if they need to.
- **The contract table is the actual deliverable of this handoff type.**
  Be exact about types, nullability, and constraints — this is what the
  receiving workstream will build against without further discussion.
- **Flag decisions, don't make them for the receiving owner.** If there's a
  genuine judgment call that belongs to whoever owns the receiving side
  (e.g., which of two possible uniqueness constraints to keep, how to name
  a field to match their existing conventions), say so explicitly as a
  flagged decision rather than picking silently on their behalf — a silent
  choice here is easy for them to miss and hard to unwind once code depends
  on it.
- **State what changes and what doesn't.** Naming the module(s) that must
  stay unchanged (a "pure" module the contract wraps, for instance) tells
  the receiving workstream exactly where their adapter code belongs and
  where it doesn't.

## Choosing between shapes

- Routine, low-stakes phase boundary, nothing unusual to flag → **Shape 1**.
- Anything with real decisions, a verification story worth spelling out, an
  imperfect handoff, or meaningful remaining risk → **Shape 2**.
- Handing a data/interface contract sideways to a workstream that hasn't
  started yet, independent of whether a whole phase is closing → **Shape 3**
  (can be combined with Shape 1 or 2 if a phase is also closing at the same
  time — write the phase handoff first, then append or link the contract
  section).
