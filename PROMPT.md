# Evidence ledger and phase handoff: paste-in prompt

Paste this into ChatGPT, Claude.ai, Gemini or a local model together with your work notes. Replace the bracketed parts.

---

You keep two records for multi-session work. Produce whichever I ask for, using only the material I give you.

## A. Evidence ledger (after analysis or review whose claims others will rely on)

Fill these fields:

1. files_consulted: every file or source actually opened this pass, with what you took from it. List only what you opened yourself.
2. files_not_accessed: what you did not read or check, and why.
3. project_claims: each claim with a status and its evidence. Allowed statuses only:
   - verified: checked this pass, with evidence you can point to.
   - asserted_unverified: stated by a source or person, not checked by you.
   - partial: only part of the claim was checked; say which part.
   - planned: not done yet.
   - retired: no longer applies.
   - unsupported_open: no evidence either way; write evidence: none.
4. external_sources: outside sources relied on.
5. conflicts_or_unknowns: conflicts between sources, open questions, things that need a human.
6. handoff_notes: what the next reader must know, and the cheapest checks that would upgrade a weak claim.

Rules: never mark a claim verified unless you saw the evidence. Never imply a skipped item was checked. If you revise an earlier ledger, write a new dated version and keep the old one.

## B. Phase handoff (when closing a phase, workstream or session)

Write a brief a fresh session can resume from with no other context:

1. goal: the project goal in two sentences.
2. state: what is done, what is in progress, what is not started.
3. decisions: choices made and the reason for each.
4. changed: files or areas changed.
5. verification: what was tested or checked, and the result.
6. risks_and_gates: remaining risks and anything that needs approval before continuing.
7. next_prompt: a ready-to-paste prompt for the next session, naming the files to read first and the first action.

Rules: assume nothing else survives. Use exact file names. State what you did not verify.

---

Task: [ledger | handoff]
Material: [paste notes, file list, test output]
