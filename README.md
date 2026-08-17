# Evidence Ledger & Phase-Handoff Convention

A lightweight documentation convention for any workflow where multiple
agents, sessions, or people build on each other's work: a **research
pipeline**, a **multi-phase build**, a **review chain**, or any project
where "what did the last session actually check" and "what does the next
session need to know" keep getting lost between hand-offs.

It gives you two small, self-contained artifacts:

- **Evidence ledgers** — a fixed-field record attached to any piece of
  analysis or deliverable work, stating exactly what was consulted, what
  was deliberately skipped, what status each claim carries (verified vs.
  merely asserted vs. unknown), and what's unresolved. This turns "I trust
  this because someone said so" into "I trust this because I can see
  exactly what was checked, by whom, and how firmly."
- **Phase handoffs** — a compact brief written at the close of a phase,
  workstream, or session that lets a new agent or session resume with
  *only this document* — no assumption that any other context survives.

Both are Markdown conventions. No tooling, no build step, no dependencies —
just a template and a short set of disciplines that make the template
actually work in practice (cite only what you opened yourself, keep claim
statuses in a closed vocabulary, never silently overwrite a ledger another
artifact may already cite, and so on).

This convention grew out of orchestrating scholarly research and writing
work with delegated AI agents, where keeping an honest audit trail of "what
was actually verified" across many parallel and sequential passes mattered
more than usual — but nothing about it is specific to that domain. It works
equally well for a human-only team handing off a project in phases, or a
mixed team of humans and agents.

## Installation

This is an **Agent Skill** — a portable folder following the `SKILL.md`
convention. To use it with an agent runtime that supports that convention
(including Claude Code), copy this folder into that runtime's skills
directory:

```sh
# Claude Code
cp -r evidence-ledger-and-phase-handoff-convention .claude/skills/

# Any other runtime supporting the SKILL.md convention
cp -r evidence-ledger-and-phase-handoff-convention <your-runtime's-skills-dir>/
```

The agent then picks it up automatically when a task matches — finishing
a piece of analysis whose claims another session will rely on, or closing
out a phase/workstream/session for someone else (or your future self) to
continue.

No script is bundled with this skill; it is a documentation/prose
convention, not a tool. Nothing needs to be installed beyond the folder
itself.

## What's inside

```
evidence-ledger-and-phase-handoff-convention/
├── SKILL.md                                 # the full convention: when to use each artifact,
│                                             # the core disciplines, both templates, self-check lists
├── references/
│   ├── evidence-ledger-template.md          # full field-by-field template + guidance
│   └── phase-handoff-template.md            # all three handoff shapes + guidance
├── examples/
│   ├── evidence-ledger-example.md           # complete synthetic worked example
│   └── phase-handoff-example.md             # complete synthetic worked example
├── LICENSE
└── README.md
```

## Worked example

The examples above use a fabricated project ("Nightjar Reading Tools") so
you can see the templates filled in end to end. A short taste — a verified
claim vs. one deliberately left open, from the evidence-ledger example:

```text
project_claims:
  - claim: "The citation resolver's fixture suite currently passes 34 of 41
    cases (82.9%)."
    status: verified
    evidence: test run output, this pass — see files_consulted above

  - claim: "The resolver correctly handles all book-chapter citation forms."
    status: unsupported_open
    evidence: none — only the fixtures currently in the suite were checked;
    this task makes no claim about citation forms the fixture suite doesn't
    cover
```

Note the shape: the first claim is backed by something this pass actually
ran and can point to; the second claim is one nobody has evidence for
either way, and the ledger says so outright instead of quietly implying
it's fine. That's the whole discipline in miniature — see
`examples/evidence-ledger-example.md` and
`examples/phase-handoff-example.md` for the full versions, and `SKILL.md`
for the reasoning behind each field.

## License

MIT — see [LICENSE](./LICENSE).
