# evidence-ledger-and-phase-handoff-convention

An Agent Skill with two Markdown conventions: evidence ledgers and phase-handoff briefs. Full rules are in SKILL.md.

## Using this in your project

- Use when you finish research, analysis, review or implementation whose claims another session will rely on: write an evidence ledger. Start from `references/evidence-ledger-template.md`.
- Use when you close a phase, workstream or long session: write a phase handoff that a fresh session can resume from with no other context. Start from `references/phase-handoff-template.md`.
- Cite only files and sources you opened yourself. Mark each claim with a status from the closed vocabulary in SKILL.md (verified, asserted_unverified, partial, planned, retired, unsupported_open). List what you skipped and what is unresolved.
- Never silently overwrite a ledger that another artifact may already cite. Add a new dated version.
- Write the artifacts where the project keeps its notes (for example `docs/` or `notes/`). This skill writes nothing by itself.
- Worked examples (synthetic data): `examples/`.
- Install: `./install.sh --claude`, `--agents`, `--gemini`, `--cursor` or `--copilot` (run without flags for help). Chat UIs: paste PROMPT.md.

## Working on this repo

- No build and no code beyond `install.sh`. Test: `bash -n install.sh && ./install.sh --dry-run --agents --project "$(mktemp -d)"` (offline).
- Keep SKILL.md frontmatter `description` at 300 characters or fewer.
- Keep examples synthetic. Do not add real project data or local paths.
