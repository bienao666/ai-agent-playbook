# Core Agent Files Template

Use this file only during initialization or compatible upgrade.

## `.agents/manager.md`

```md
# Manager Agent

## Responsibilities

- 澄清需求、扫描项目、判断复杂度和生命周期阶段。
- 拆分任务并定义验收标准。
- 调度 Builder / Reviewer / 临时 Agent。
- 维护 task log、lifecycle loop、decisions、lessons。
- 最终汇总交付，不跳过必要验收。

## 管理决策

任务编号:
目标:
分配对象:
原因:
验收标准:
下一步:
```

## `.agents/builder.md`

```md
# Builder Agent

## Responsibilities

- 按 Manager 分配实现最小必要变更。
- 遵循项目现有架构、风格、测试方式。
- 不做无关重构，不扩大范围。
- 如启用 agent-skills，遵循当前生命周期阶段。
- 如启用 Ponytail，先完成最小实现判断。

## Builder Report

任务编号:
状态:
修改文件:
生命周期阶段:
实现说明:
已运行测试:
Ponytail Check:
最小实现取舍:
风险:
问题:
```

## `.agents/reviewer.md`

```md
# Reviewer Agent

## Responsibilities

- 以代码审查姿态检查结果。
- 优先发现 bug、回归、安全、边界、缺失测试。
- 检查是否满足验收标准和生命周期阶段要求。
- 如启用 Ponytail，检查过度设计和无必要依赖。

## Review Result

任务编号:
结论: Pass | Needs Changes
生命周期阶段:
发现:
必须修改:
测试缺口:
Ponytail Review:
过度设计风险:
剩余风险:
```

## `.agents/protocol.md`

```md
# Protocol

## Task Assignment

任务编号:
发送方:
接收方:
目标会话:
目标:
上下文:
约束:
验收标准:
Capability:
Reason:
Fallback:
Agent Skills:
Skill Reason:
Skill Fallback:
Ponytail:
Ponytail Reason:
Ponytail Fallback:
回复给:

## Task Report

任务编号:
发送方:
接收方:
来源会话:
状态:
生命周期阶段:
变更:
测试:
风险:
问题:

## Review Request

任务编号:
发送方:
接收方:
目标会话:
范围:
修改文件:
关注点:
验收标准:

## Review Response

任务编号:
发送方:
接收方:
来源会话:
结论:
发现:
必须修改:
```

## `.agents/task-log.md`

```md
# Task Log

| 任务编号 | 状态 | 负责人 | 生命周期阶段 | 摘要 | 创建时间 | 更新时间 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- |

状态只能使用：Proposed, Assigned, In Progress, Reviewing, Needs Changes, Done, Blocked。
```

## `.agents/handoff.md`

```md
# Handoff

- 每个 Agent 必须说明已完成、未完成、下一步建议。
- 长任务更新 task log 和 lifecycle loop。
- 重要决策记录原因。
- 交接只传最小必要上下文和当前生命周期阶段。
```

## `.agents/review-checklist.md`

```md
# Review Checklist

- 是否满足验收标准。
- 是否遵循项目现有模式。
- 是否符合当前 agent-skills 生命周期阶段。
- 是否保留用户已有改动。
- 是否处理边界、错误、安全、隐私、数据丢失风险。
- 是否运行相关测试，或说明无法运行原因。
- 是否存在不必要依赖、抽象、文件、配置、无关重构。
- 最终回复是否清楚说明完成内容、测试和剩余风险。
```

## `AGENTS.md` section

If `AGENTS.md` exists, append only missing rules.

```md
## Project Rules

- Read the existing codebase before making changes.
- Prefer existing project patterns over new abstractions.
- Keep changes scoped to the task.
- Do not revert user changes unless explicitly requested.
- Run relevant tests when possible.
- Report tests that could not be run.

## Multi-Agent Collaboration

- Multi-Agent Mode 默认关闭；先检查 `.agents/settings.md` 和 `.agents/local-settings.md`。
- 启用后用户无需重复说“按多 Agent 流程”。
- 每次交互先输出精简中文 `管理决策`。
- Simple 任务可由 Manager 自检；`reviewer_for_simple` 开启时也交 Reviewer。
- Medium / Complex / High Risk 任务使用 Builder / Reviewer。
- 真实 sub-agent 只有 `real_subagents` 或 `real_subagents_default` 为 `on` 时才自动创建。
- 子智能体按任务创建、按任务关闭，名称包含角色和任务号。
- agent-skills 可用时按 `/spec`、`/plan`、`/build`、`/test`、`/review`、`/webperf`、`/code-simplify`、`/ship` 路由。
- 按需发现 Superpowers、agent-skills、skills、plugins、MCP tools、Ponytail 和本地脚本。
- 重要决策写入 `.agents/decisions.md`，经验写入 `.agents/lessons.md`。
```
