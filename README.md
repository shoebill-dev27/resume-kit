# resume-kit

Two lightweight, **read-only** skills to re-orient at the start of a Claude Code session.
English command names, bilingual (EN / JA) natural-language triggers — so it's easy to share.

| Command | Triggers (EN) | Triggers (JA) | What it does |
|---------|---------------|---------------|--------------|
| `/whereami` | "where are we", "current state", "status" | 「どこまでやったっけ」「今どういう状況」 | Reports project state, then **stops**. |
| `/resume`   | "resume", "continue", "pick up" | 「再開しよう」「続きから」 | Orients, proposes the next step, and **continues after you confirm**. |

Both gather: `git status` / recent & unpushed commits / open PR, project intent docs
(`CLAUDE.md`, `STATUS.md`, `TODO.md`, …), the last session transcript under
`~/.claude/projects/`, and user memory — then print a **Done / In progress / Next / Open** summary.

## Install

```sh
/plugin marketplace add shoebill-dev27/resume-kit   # or a local path to this repo
/plugin install resume-kit@resume-kit-marketplace
```

## Layout

```
resume-kit/
├── .claude-plugin/
│   ├── plugin.json        # plugin manifest
│   └── marketplace.json   # lets people `marketplace add` this repo directly
└── skills/
    ├── whereami/SKILL.md
    └── resume/SKILL.md
```

> Plugin/marketplace schemas evolve; verify against the current Claude Code docs before publishing.
