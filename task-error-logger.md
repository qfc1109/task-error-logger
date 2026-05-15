---
name: task-error-logger
description: Use when maintaining a project task_plan.md, recording task progress, bug or error analysis, issue logs, configuration changes, code changes, rollback notes, verification results, or when the user asks to update a task plan, task log, error log, bug record, or work journal.
---

# Task Error Logger

## Purpose

Maintain `task_plan.md` as a concise, date-based engineering log. Record what was done, why it was done, what changed, how to roll it back, and what was verified.

## Operating Rules

- Use UTF-8 for all reads and writes.
- Preserve existing content. Add a new dated section above older dates unless the user explicitly asks to edit an existing section.
- Do not guess facts. If required information is missing, ask one concise question and wait.
- If the user says to update the log after completed work and enough context is available, update the file directly.
- Ask questions only when needed to avoid false records. Do not run a mandatory confirmation ritual for routine log updates.
- Use the workspace root `task_plan.md` unless the user provides another path.
- Keep entries specific enough for a later engineer to understand the change and roll it back.

## Workflow

1. Read the current `task_plan.md` with UTF-8.
2. Determine today’s date from the environment.
3. Add or update today’s section at the top of the file after `# 任务计划` and the first `---`.
4. Use append-style entries inside the date section. Do not mix work from different dates.
5. Record verification evidence only if commands were actually run. If verification was blocked, record the blocker.
6. Re-read the changed file with UTF-8 and confirm the new section renders normally.

## Section Template

Use these headings for each date. Omit a heading only when it is clearly irrelevant and there is no content for it.

```markdown
---

## YYYY-MM-DD

### 任务列表
- 【✔】 已完成任务描述 <YYYY-MM-DD HH:mm>
- 【 】 未完成任务描述 <YYYY-MM-DD HH:mm>

### 问题分析
- 【❓】 错误类型：NullReferenceException / 配置缺失 / 编译失败 / ...
- 位置：`path/to/file.java:123`
- 堆栈摘要：关键调用链或错误输出摘要
- 可能原因：基于证据描述；不确定时写“待确认”
- 责任方：前端 / 后端 / 配置 / 工具环境 / 需联调

### 问题记录
- 【✔】 问题描述：解决方法简述 <YYYY-MM-DD HH:mm>
- 【❓】 未解决问题描述：当前阻塞点 <YYYY-MM-DD HH:mm>
- 【→】 延伸问题描述：由本次处理发现的新问题 <YYYY-MM-DD HH:mm>

### 配置修改
| 原值 | 新值 | 说明 |
|------|------|------|
| `old` | `new` | 修改原因 |

### 代码修改
| 文件 | 修改内容 | 回滚方法 |
|------|----------|----------|
| `relative/path/File.java` | 原代码：`oldCode`。新代码：`newCode`。影响：简述行为变化。 | 恢复原代码或删除新增代码 |
```

## Status Marks

| 标记 | 含义 |
|------|------|
| `【 】` | 计划中或未完成 |
| `【✔】` | 已完成或已解决 |
| `【❓】` | 待确认、待定位或仍阻塞 |
| `【→】` | 延伸问题或联调后发现的问题 |

## Code Change Records

For every code change row, include:

- Full relative path from the workspace root.
- What changed.
- Original code or original behavior when applicable.
- New code or new behavior when applicable.
- Rollback instructions that a later engineer can execute.

If the exact original code is unavailable, write `原代码：未记录` and explain why.

## Responsibility Rules

| Evidence | Responsibility |
|----------|----------------|
| `.java`, server logs, backend stack trace | 后端 |
| `.cs`, `Assets/Scripts`, Unity/client stack trace | 前端 |
| XML, properties, xlsx/config data, GN tables | 配置 |
| Missing local tools, PATH, dependency cache, unavailable services | 工具环境 |
| Backend fix exposes client-side failure | 需联调, record with `【→】` |

## Common Cases

- **Record completed coding work:** Add today’s task list, issue record, code change table, and verification result.
- **Record a bug:** Add `问题分析` with type, location, stack summary, likely cause, and responsibility.
- **Record blocked verification:** Add `问题记录` with the exact command/tool that failed and why.
- **Update an existing task:** Change `【 】` to `【✔】` and append completion time. Preserve the original task text.

## Safety Checks

Before final response:

- Confirm the newest date section is above older sections.
- Confirm old sections were not overwritten.
- Confirm file paths are relative to the workspace root.
- Confirm any verification claim has command evidence.
- Confirm UTF-8 rendering is readable.
