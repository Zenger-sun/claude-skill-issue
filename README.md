# claude-skill-issue

适用于 [Claude Code](https://claude.ai/code) 的自定义 skill，用于在对话之间持久化归档和追踪技术问题。

[English](README.en.md)

## 功能

- `/issue archive` — 扫描当前对话，提取技术问题（bug、兼容性、配置错误等），写入持久化问题库，自动去重合并相同/相似问题。
- `/issue enable` — 启用自动查阅：每次对话开始时自动加载历史问题库，让 Claude 具备跨对话的问题上下文。
- `/issue disable` — 禁用自动查阅。
- `/issue` — 不带子命令时默认执行 archive。

## 安装

**第一步**：将 `.claude/commands/issue.md` 复制到你项目的 `.claude/commands/` 目录：

```
your-project/
└── .claude/
    └── commands/
        └── issue.md   ← 复制此文件
```

**第二步**：将本仓库 `CLAUDE.md` 中的内容追加到你项目的 `CLAUDE.md`，启用自动查阅机制。

## 使用方法

```
/issue archive    # 归档当前对话中的技术问题
/issue enable     # 启用对话开始时自动读取问题库
/issue disable    # 禁用自动读取
/issue            # 同 archive（默认行为）
```

典型工作流：

1. 解决完一个 bug 后，运行 `/issue archive` 将问题和解法存入库
2. 运行 `/issue enable` 开启自动查阅
3. 后续对话中 Claude 会自动参考历史问题，避免重复踩坑

## 数据文件

skill 在项目根目录下读写以下文件（首次 `/issue archive` 时自动创建）：

```
.claude/issues/
├── config.json   # {"enabled": true/false}
├── index.md      # 问题列表（状态、引用次数）
└── detail.md     # 问题详情（描述、根本原因、解决办法）
```

本仓库 `.claude/issues/` 下提供了示例文件，展示预期格式。

## 问题状态

| 标记 | 含义 |
|------|------|
| ✅ 已解决 | 问题已有明确解法 |
| ❌ 未解决 | 问题尚未解决 |
| ⚠️ 修复中 | 正在处理中 |

## 依赖

- [Claude Code](https://claude.ai/code)，需支持自定义 skill（`/commands`）
- skill 使用 `Read`、`Write`、`Bash` 工具权限
