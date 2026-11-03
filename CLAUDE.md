# CLAUDE.md

## Model routing

Follow `docs/MODEL_STRATEGY.md`. In short:

- Mechanical tasks (commit known files, move or delete files, run a command and report, status checks, checklists) go to the `quick-task` subagent (Haiku).
- Small, well-scoped coding or writing tasks go to the `standard-task` subagent (Sonnet).
- Hard design work, unclear bugs, and costly decisions stay in the main session.
- If unsure, start at Standard.

## Session hygiene

- One task per session. Start fresh when a task is done.
- Start a fresh session with a short summary once context passes about 300k tokens.
- Don't leave a session blocked on a human step. Write the step down and close the session.
