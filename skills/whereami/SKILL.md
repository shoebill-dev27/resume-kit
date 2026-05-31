---
name: whereami
description: Read-only orientation for the current project — "where do things stand right now?" Use when the user asks "whereami", "where are we", "what's the current state", "status", or in Japanese 「どこまでやったっけ」「今どういう状況」「現状は」. Gathers git state, the last session transcript, and progress notes, then prints a Done / In progress / Next / Open summary. Never edits, commits, pushes, or starts work — it only reports.
---

# whereami — where do things stand?

Goal: in one pass, gather the project's recent state and present a tight **Done / In progress / Next / Open** summary. This skill is **read-only**: never edit, stage, commit, push, or begin implementation. Stop after the report.

## Steps

1. **Identify the project.** Run `pwd` and `git rev-parse --show-toplevel`. Use the git root as the project. If not in a git repo, use `pwd` and skip the git steps.

2. **Gather state — run these in parallel:**
   - `git status -sb` — branch, uncommitted/untracked files, ahead/behind.
   - `git log --oneline -15` — recent commits.
   - `git log --oneline @{u}.. 2>/dev/null` — commits not yet pushed (ignore the error if there is no upstream).
   - `gh pr list --head "$(git branch --show-current)" 2>/dev/null` — open PR for this branch, if `gh` exists.

3. **Read project intent docs** (only those that exist): the project's `CLAUDE.md`, and any `STATUS.md` / `PROGRESS.md` / `TODO.md` / `ROADMAP.md` / `docs/STATUS.md`. These hold the human-stated "where we are".

4. **Recall the last session.** Convert the project's absolute path to a transcript dir name by replacing every `/` with `-` (e.g. `/home/you/dev/app` → `-home-you-dev-app`) and look under `~/.claude/projects/<that-name>/`. Find the most recent `*.jsonl` by mtime (exclude the current session if identifiable) and read its **last ~40 lines** to recover the final user requests and what was in flight. If a "This session is being continued…" summary block exists, prefer it.

5. **Check user memory**, if present. Look for a memory index at `~/.claude/projects/*/memory/MEMORY.md` and open any linked memory whose description matches this project — it may hold the canonical status (phase, design-review state, etc.).

## Output

Lead with a one-line status, then the four buckets. Keep it scannable — bullets, not prose. Omit a bucket if genuinely empty.

```
📍 <project> — <branch>, <N uncommitted, M unpushed / clean>

✅ Done (recent)
- <from commits + status docs>

🔧 In progress
- <uncommitted work + last session's in-flight task>

➡️ Next
- <stated next step from docs / memory / last session>

❓ Open / blockers
- <unresolved questions, failing checks, decisions pending>
```

Then stop. Do **not** propose to start work — that is `/resume`'s job.

## Notes
- Trust git and the last transcript over memory when they conflict; memory reflects a past moment.
- If signals conflict (a doc says "done" but commits/tests suggest otherwise), surface the discrepancy rather than smoothing it over.
- Respect the user's CLAUDE.md style: conclusion first, concise, and label guesses as guesses.
