# Maintaining Task Plan

`maintaining-task-plan` 是一个用于维护 `task_plan.md` 的技能。它的目标不是写很长的流水账，而是让任务计划先给人看得懂：当前做什么、下一步做什么、未结束需求卡在哪个阶段。

远程仓库：[qfc1109/maintaining-task-plan](https://github.com/qfc1109/maintaining-task-plan.git)

## 适用场景

- 记录或更新 `task_plan.md`。
- 记录需求阶段：待办、进行中、阻塞、完成。
- 整理过长、过乱、看不懂的任务计划。
- 记录用户反馈的 bug 或问题。
- 记录某一天做的事属于哪个需求、哪个阶段。

## 当前记录原则

- `需求索引` 只放未结束、后面还要继续跟的需求。
- 已完成需求不放在 `需求索引`。
- `需求详情` 只展开当前索引里的需求。
- 已结束需求只保留在日期记录或 `历史日期记录` 中用于查证。
- 日期记录必须能追到 `需求ID / 阶段`。
- 长期总结写 `progress.md`，问题和风险写 `findings.md`，正式计划写 `docs/plans/*.md`。

## 仓库结构

```text
.
├── agents/
│   └── openai.yaml
├── SKILL.md
└── README.md
```

## 文件说明

- `SKILL.md`：技能主体，包含触发说明、任务计划结构、状态标记和安全检查。
- `agents/openai.yaml`：OpenAI agent 入口配置，定义显示名称、简介和默认提示词。
- `README.md`：仓库说明文档。

## 使用方式

显式调用：

```text
$maintaining-task-plan
```

也可以直接描述需求：

```text
更新 task_plan.md：Unity 客户端阶段 1 正在进行，已经检查了 Unity 环境，当前卡在本机没装 Editor，下一步安装 Unity 6 LTS。
```

```text
整理 task_plan.md，已经结束的需求不要继续放在需求索引里。
```

## 维护建议

- 修改技能主体时，优先更新 `SKILL.md`。
- 如果默认调用文案变化，同步更新 `agents/openai.yaml`。
- 部署到本机 Codex 时，同步到 `C:\Users\Administrator\.codex\skills\maintaining-task-plan\SKILL.md`。
- 推送前确认 `origin` 指向 `https://github.com/qfc1109/maintaining-task-plan.git`。
- 提交前检查 Markdown、状态标记和 `需求ID / 阶段` 示例是否正常。
