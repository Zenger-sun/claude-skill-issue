# Issue Skill for Claude Code

## Issue 自动查阅

每次对话开始时，读取 `.claude/issues/config.json`：
- 若 `enabled` 为 `true`，立即读取 `.claude/issues/index.md` 和 `.claude/issues/detail.md`，将历史问题作为背景上下文，在回答相关问题时主动参考。
- 若 `enabled` 为 `false` 或文件不存在，跳过。

使用 `/issue enable` 或 `/issue disable` 控制此行为。
