---
name: task-error-logger
description: Use when maintaining task_plan.md, 任务计划, 阶段计划, 需求执行日志, task logs, or work journals; when users ask to 记录进度, 更新进度, 记录阶段, 更新阶段状态, 整理 task_plan.md, 记一下当前做到哪, 标记待办/进行中/阻塞/完成, 记录bug, 问题记录, or record what requirement and phase a date belongs to.
---

# Task Error Logger

## Purpose

Maintain `task_plan.md` as a human-readable requirement and phase log.

The file must first answer:

1. 现在正在做什么？
2. 下一步做什么？
3. 当前没结束的需求分别卡在哪个阶段？

Date records are only evidence. They are not the main navigation.

## Core Rules

- Use UTF-8 for all reads and writes.
- Use the workspace root `task_plan.md` unless the user provides another path.
- Keep the top of the file simple enough for a human to read quickly.
- `需求索引` only lists unfinished requirements that still need tracking.
- Completed requirements must not stay in `需求索引`.
- `需求详情` only expands requirements currently listed in `需求索引`.
- Completed historical requirements belong under date records, preferably compacted under `历史日期记录`.
- Every date log bullet that mentions a requirement must include `需求ID / 阶段`.
- Do not create free-floating `当前阶段`, `阶段计划`, `决策记录`, broad summaries, or duplicate navigation blocks at the bottom.
- Long-term summaries belong in `progress.md`; findings and risks belong in `findings.md`; formal plans belong in `docs/plans/*.md`.

## File Shape

Use this shape by default:

```markdown
# 项目任务计划

## 当前目标

一句话说明当前正在做什么。下一句说明确认后下一步做什么。

## 需求索引

只放还没结束、后面还要继续跟的需求。已经完成的历史需求不放这里，只留在日期记录里查证。

| 需求ID | 需求 | 当前状态 | 当前阶段 | 下一步 |
|--------|------|----------|----------|--------|
| R-20260606-unity-client | 先做 Unity 客户端真实原型 | 待继续 | 阶段 1：检查环境并创建工程 | 创建 Unity 工程和第一张可运行场景 |

## 需求详情

### R-20260606-unity-client：先做 Unity 客户端真实原型

- 目标：先把 Unity 客户端跑起来，再决定后端需要哪些接口、字段、配置和数据库设计。
- 计划文档：`docs/plans/...md`
- 当前：还没开始真实 Unity 工程。

#### 阶段

- 【✔】 阶段 0：生成客户端开发计划；结果：已明确阶段拆分 <2026-06-06>
- 【→】 阶段 1：检查 Unity 环境并创建真实 Unity 工程；已完成：...；当前停在：...；下一步：...
- 【 】 阶段 2：创建场景、玩家、敌人和相机

## 2026-06-06

### 执行记录

- 【→】 R-20260606-unity-client / 阶段 1：检查 Unity 环境；当前处理：正在确认本机是否安装 Unity Editor；下一步：创建工程或记录阻塞 <2026-06-06>

### 问题记录

- 【✔】 R-20260606-unity-client / 阶段 0：用户反馈“前端 Unity 还没有场景、特效”；处理结果：生成客户端优先计划 <2026-06-06>

### 变更记录

| 需求ID | 阶段 | 文件/配置 | 修改内容 | 回滚方法 |
|--------|------|-----------|----------|----------|
| R-20260606-unity-client | 阶段 0 | `docs/plans/...md` | 新增客户端计划 | 删除该文档 |

## 历史日期记录

这里仅用于查证过去做过什么，不作为当前计划入口。

### 2026-06-05

- 【✔】 R-20260605-core-config / 阶段 1：完成后端第一链路 core 下沉与配置驱动 <2026-06-05>
```

## Status Marks

| 标记 | 含义 |
|------|------|
| `【 】` | 待办：阶段尚未开始 |
| `【→】` | 进行中：阶段已开始，记录已完成、当前停在何处、下一步 |
| `【❓】` | 阻塞：阶段无法继续，记录阻塞点、已尝试、需要什么 |
| `【✔】` | 完成：阶段已完成，记录结果、影响范围、验证证据或未验证原因 |

## Workflow

1. Read `task_plan.md` with UTF-8. If it does not exist and the user asked to maintain it, create it.
2. Determine today's date from the environment.
3. Identify the active requirement:
   - Continuing existing work: reuse its `需求ID`.
   - New unfinished objective: create `R-YYYYMMDD-short-slug`.
   - Completed objective: do not add it to `需求索引`; record it only in date logs.
4. Ensure every unfinished requirement appears in `需求索引`.
5. Ensure `需求详情` contains only requirements from `需求索引`.
6. Update the matching phase under `需求详情`.
7. Add or update today's date section near the top of date logs.
8. Add `执行记录`, `问题记录`, or `变更记录` entries. Each requirement-related date bullet must use `需求ID / 阶段`.
9. If a requirement is completed, record completion in today's date log, then remove it from `需求索引` and `需求详情`.
10. Re-read the file and confirm it remains human-readable.

## Requirement Index Rules

`需求索引` is not a history table.

Keep in `需求索引`:

- `进行中`
- `待继续`
- `阻塞`
- `暂停` if the user still expects it to resume soon

Do not keep in `需求索引`:

- `完成`
- one-off cleanup already accepted by the user
- old requirements kept only for audit/history

If unsure whether a paused requirement still needs tracking, prefer not to clutter the index. Keep it in history and restore it later if the user resumes it.

## Phase Entry Rules

Keep phase names stable. Change only status and trailing details.

- `【 】` entries state the phase goal.
- `【→】` entries must include `已完成`, `当前停在`, and `下一步`.
- `【❓】` entries must include `阻塞点`, `已尝试`, and `需要`.
- `【✔】` entries must include `结果`, `影响范围`, and `验证` or `验证：未运行，原因：...`.

Good:

```markdown
- 【→】 阶段 2：接入 WebSocket 登录；已完成：客户端可连接本地网关；当前停在：`REGION_SNAPSHOT` 缺实体详情；下一步：记录后端字段缺口 <2026-06-06>
```

Bad:

```markdown
- 【→】 阶段 2：继续开发 <2026-06-06>
```

## Date Log Rules

Date records are evidence, not navigation.

- Today's active work can use `## YYYY-MM-DD`.
- Older compacted records can live under `## 历史日期记录` with `### YYYY-MM-DD`.
- Date bullets that mention a requirement must include `需求ID / 阶段`.
- Do not expand every old completed requirement into `需求详情`.
- Preserve enough historical evidence to understand what happened and where to find the formal plan.

Good:

```markdown
- 【✔】 R-20260606-unity-client / 阶段 0：生成客户端优先开发计划；结果：新增 `docs/plans/...md` <2026-06-06>
```

Bad:

```markdown
- 【✔】 生成客户端优先开发计划 <2026-06-06>
```

## Bug Record Rules

Bug records must also reference a requirement and phase when possible.

- Prefer the user's own wording.
- If the phase is obvious, write `需求ID / 阶段 N`.
- If not obvious, write `关联阶段：待确认` and do not invent a link.
- Keep bug records lightweight unless the user asks for root-cause analysis.

Example:

```markdown
- 【❓】 R-20260606-unity-client / 阶段 4：用户反馈“连接后没有显示敌人”；当前处理：排查中 <2026-06-06>
```

## Change Record Rules

Use `变更记录` only when files, config, generated artifacts, or data changed.

Columns:

```markdown
| 需求ID | 阶段 | 文件/配置 | 修改内容 | 回滚方法 |
|--------|------|-----------|----------|----------|
```

Rules:

- File paths must be relative to the workspace root.
- Record only changes relevant to the requirement.
- Do not record unrelated refactors or user changes.
- Rollback instructions should be executable without reading chat history.

## Cleanup Rules

When users say the file is confusing, too complex, too long, or hard to understand:

- Keep `当前目标` short.
- Keep `需求索引` to unfinished requirements only.
- Keep `需求详情` to requirements in the index only.
- Move completed requirements out of index/details and into compact date history.
- Remove duplicate navigation blocks and broad summaries.
- Preserve dated execution evidence.

## Safety Checks

Before final response:

- Confirm there is one `当前目标`, one `需求索引`, and one `需求详情`.
- Confirm completed requirements are not listed in `需求索引`.
- Confirm every requirement in `需求索引` has a matching `需求详情` section.
- Confirm no extra completed requirement details remain under `需求详情`.
- Confirm date bullets that mention a requirement include `需求ID / 阶段`.
- Confirm any `【→】` phase has `当前停在` and `下一步`.
- Confirm any `【❓】` phase has `阻塞点` and `需要`.
- Confirm UTF-8 rendering is readable.
