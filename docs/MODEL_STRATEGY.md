# Model Strategy

Goal: spend the most capable model only where it changes the outcome.

## The three tiers

| Tier | Model | Use for | Analogy |
|---|---|---|---|
| Fast | `claude-haiku-5-5` | Mechanical work with a clear answer: file moves, commits, renames, lookups, formatting, status checks, device steps | A checkout clerk: quick, accurate on routine items |
| Standard | `claude-sonnet-5-5` | Most coding and writing: small features, bug fixes, edits across a few files, drafting | A competent mid-level engineer |
| Deep | `claude-opus-5-5` | Hard problems: architecture, tricky bugs with unclear cause, financial or legal reasoning, long multi-step plans | A senior specialist you call in for hard cases |

Default to the lowest tier that can do the job. Move up only when a lower tier fails or the task clearly needs more reasoning.

## Routing rules

**Use Fast (Haiku) for:**
- Committing or staging known files
- Deleting, renaming, or moving files
- Running a command and reporting its output
- Reading a file and answering a direct question
- Following a written checklist (for example, a device trust step)
- Status checks on PRs, CI, or sessions

**Use Standard (Sonnet) for:**
- Small features or bug fixes with a known location
- Edits across a few files
- Writing or revising documents, copy, or blog posts
- Reviewing a diff against a clear checklist

**Use Deep (Opus) for:**
- Designing a system or choosing between approaches with real trade-offs
- Debugging when the cause is unknown after one or two attempts
- Financial categorization rules, tax or legal reading, anything where a mistake is costly
- Planning multi-step work that spans several repos

**If unsure, start at Standard.** Escalate only when the result is wrong or incomplete.

## Session hygiene

Cost grows with context. Every turn re-reads the whole conversation.

1. **One task per session.** Start a new session when the task is done. Don't reuse a session for an unrelated job.
2. **Watch context size.** Past about 300k tokens, start a fresh session and carry over a short summary.
3. **Archive finished work.** Archive sessions once their result is merged or delivered.
4. **Don't leave sessions blocked.** If you're waiting on a human step, write the exact step down and close the session. Reopen when you're ready.
5. **Use subagents for side tasks.** Send a small, self-contained question to a subagent. Its context stays separate from the main session.

## Subagents in this repo

Defined in `.claude/agents/`:

- `quick-task` (Haiku): mechanical, well-defined work
- `standard-task` (Sonnet): small coding and writing tasks

The main session delegates to them with the Agent tool. Their context is isolated, so the main conversation stays short.

## Review cadence

Once a month, check the account's sessions for:
- Opus sessions doing Fast-tier work
- Sessions above 400k context
- Blocked sessions older than a week
