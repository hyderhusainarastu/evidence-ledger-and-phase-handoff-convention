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

This is an **Agent Skill**: a portable folder following the `SKILL.md`
convention. No script runs and nothing else needs installing.

| Surface | How | Status |
|---|---|---|
| Claude Code | `./install.sh --claude` (symlinks into `~/.claude/skills/`) | not tested |
| Codex | `./install.sh --agents --project DIR` (`.agents/skills/`) | not tested |
| Gemini CLI | `./install.sh --agents --project DIR` or `--gemini` pointer block | not tested |
| Cursor | `./install.sh --agents --project DIR` (reads `.agents/skills/`) | not tested |
| Copilot | `./install.sh --copilot --project DIR` (pointer block) | not tested |
| ChatGPT, Claude.ai, Gemini (chat) | paste [PROMPT.md](./PROMPT.md) | not tested |

Run `./install.sh` with no flags to see options and detected tools. Add
`--copy` to copy instead of symlink, `--dry-run` to preview and
`--uninstall` to remove only what the installer created.

The agent picks the skill up when a task matches: finishing analysis whose
claims another session will rely on, or closing out a phase, workstream or
session for someone else (or your future self) to continue.

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
├── AGENTS.md, CLAUDE.md, GEMINI.md        # agent instructions (pointers to AGENTS.md)
├── PROMPT.md                                # paste-in version for chat UIs
├── install.sh                               # per-platform installer
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

## Related

- [cross-session-agent-handoff-prompt-template](https://github.com/hyderhusainarastu/cross-session-agent-handoff-prompt-template): a copy-paste prompt for handing a task to a fresh session.
- [multi-agent-orchestration-playbook](https://github.com/hyderhusainarastu/multi-agent-orchestration-playbook): patterns for coordinating several agents; this convention covers the records they leave behind.

Keywords: agent handoff, evidence ledger, audit trail, multi-agent, context engineering, session resume, research workflow.

## License

MIT — see [LICENSE](./LICENSE).

Part of palimnote-skills - more at https://github.com/hyderhusainarastu
