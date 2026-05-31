---
name: resume
description: Re-orient on the current project and then continue the work. Use when the user asks "resume", "continue", "pick up where we left off", or in Japanese 「再開しよう」「続きから」「前回の続き」. First runs the same orientation as the whereami skill, then proposes the single next action and — once the user confirms — begins executing it.
---

# resume — pick up where you left off and continue

Goal: orient, then move. This is `whereami` plus the authority to continue working after the user confirms.

## Steps

1. **Orient.** Perform the full context-gathering and produce the **Done / In progress / Next / Open** summary exactly as described in the `whereami` skill (git state, unpushed commits, open PR, intent docs, last session transcript, user memory). Keep it tight.

2. **Propose one next action.** From that summary, pick the single most sensible next step and state it as a question — e.g. "次は X を実装して `make lint` を通すところからで良い? / Shall I start with X and get `make lint` green?"

3. **Wait for confirmation.** Do not touch files before the user agrees. A short "OK" / "go" / 「いいよ」/「進めて」 counts as confirmation.

4. **Execute.** Once confirmed, begin the work, honoring the user's CLAUDE.md rules — small focused commits, run `make lint` and tests, never push to main (feature branch → PR via `gh pr create`), and surface trade-offs when the path isn't clear.

## Difference from `whereami`
- `whereami` reports and **stops**.
- `resume` reports, proposes, and — after confirmation — **continues working**.
