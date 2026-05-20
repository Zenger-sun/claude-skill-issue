# claude-skill-issue

A Claude Code custom skill for archiving and tracking technical issues across conversations.

## What it does

- `/issue archive` — Scans the current conversation, extracts technical problems (bugs, compatibility issues, misconfigurations), and writes them to a persistent issue database. Deduplicates by merging identical/similar issues.
- `/issue enable` — Enables auto-loading of the issue database at the start of each conversation, so Claude has historical context.
- `/issue disable` — Disables auto-loading.

## Installation

Copy `.claude/commands/issue.md` into your project's `.claude/commands/` directory:

```
your-project/
└── .claude/
    └── commands/
        └── issue.md   ← copy this file
```

Then add the auto-load instruction to your project's `CLAUDE.md` (copy from `CLAUDE.md` in this repo).

## Usage

```
/issue archive    # archive issues from current conversation
/issue enable     # enable auto-load on conversation start
/issue disable    # disable auto-load
/issue            # same as archive (default)
```

## Data files

The skill reads and writes these files in your project root:

```
.claude/issues/
├── config.json   # {"enabled": true/false}
├── index.md      # issue list with status and reference count
└── detail.md     # full issue details
```

These files are auto-created on first `/issue archive`. The example files in this repo show the expected format.

## Issue states

| Symbol | Meaning |
|--------|---------|
| ✅ 已解决 | Resolved |
| ❌ 未解决 | Unresolved |
| ⚠️ 修复中 | In progress |

## Requirements

- [Claude Code](https://claude.ai/code) with custom skills support
- The skill uses `Read`, `Write`, and `Bash` tools
