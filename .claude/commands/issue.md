---
name: issue
description: 项目问题归档系统。归档当前对话中的技术问题，或启用/禁用自动查阅历史问题。子命令：archive（归档）、enable（启用自动查阅）、disable（禁用）。
allowed-tools: Read, Write, Bash
argument-hint: "[archive|enable|disable]"
effort: low
---

问题数据库路径：`.claude/issues/index.md` 和 `.claude/issues/detail.md`

**若未传入子命令（ARGUMENTS 为空），默认执行 archive。**

根据用户传入的子命令执行对应操作：

---

## archive

1. 读取 `.claude/issues/index.md` 和 `.claude/issues/detail.md`（不存在则视为空，序号从 1 开始）
2. 分析**当前对话**，识别所有技术问题（bug、兼容性、配置错误等，不含功能需求）
3. 对每个问题判断归类并操作：
   - **相同问题**：引用次数 +1；若有更根本原因则更新；若仍未解决则追加已探索方向和下一步方向
   - **相似问题**（同技术域且症状相近）：插入到相似条目之后，序号顺延
   - **新问题**：追加到末尾
4. 写回 `index.md` 和 `detail.md`

index.md 格式：
```
| 序号 | 问题简要描述 | 状态 | 引用次数 |
|------|------------|------|---------|
| 1 | xxx | ✅ 已解决 | 3 |
| 2 | xxx | ❌ 未解决 | 1 |
```

detail.md 已解决格式：
```
## #1 问题简要描述

**状态**：✅ 已解决
**详细描述**：xxx
**根本原因**：xxx
**最终解决办法**：xxx
**创建时间**：YYYY-MM-DD
**更新时间**：YYYY-MM-DD
```

detail.md 未解决格式：
```
## #2 问题简要描述

**状态**：❌ 未解决
**详细描述**：xxx
**已探索方向**：
- xxx
**下一步方向**：
- xxx
**创建时间**：YYYY-MM-DD
**更新时间**：YYYY-MM-DD
```

---

## enable

1. 写入 `.claude/issues/config.json`：`{"enabled": true}`
2. 读取 `index.md` 和 `detail.md`，将内容作为上下文摘要输出给用户，说明已启用
3. 告知用户：后续每次使用 `/issue` 时会自动参考历史问题库；若遇到与已解决问题相同的问题但历史方案无效，请运行 `/issue archive` 更新状态

---

## disable

1. 写入 `.claude/issues/config.json`：`{"enabled": false}`
2. 告知用户：已禁用，不再自动读取 issue 库

---

## 注意事项

- 序号全局唯一，不复用
- 每次更新 detail.md 条目时必须更新"更新时间"
- 当前时间通过 `date "+%Y-%m-%d %H:%M:%S"` 获取，创建时间和更新时间均精确到秒
