# Runtime Rules Template

Use this file only during initialization or compatible upgrade.

## `.agents/session-registry.md`

```md
# Session Registry

| 角色 | 会话标识 | 用途 | 状态 | 当前任务 | 最后更新 |
| --- | --- | --- | --- | --- | --- |
| Manager | current | 协调、拆解、汇总 | Active |  |  |
| Builder |  | 实现、修改、验证 | Not Created |  |  |
| Reviewer |  | 审查、风险、测试缺口 | Not Created |  |  |

## Session Policy

- Multi-Agent Mode 未启用时不创建独立会话。
- 真实 sub-agent 只在 `real_subagents` 或 `real_subagents_default` 为 `on` 时自动创建。
- 优先原生 sub-agent，其次 thread，最后同会话模拟。
- 不维护常驻 Builder / Reviewer；按任务创建、按任务关闭。
- 名称必须包含角色和任务号，例如 `Builder-T-001`。
- 创建前关闭历史遗留且当前不用的 sub-agent。
- `wait_agent` 完成后立即 `close_agent`。
- `close_agent` 返回 `not found` 记录为 `Closed/Not Found`。
- 不支持关闭时标记 `Close Unsupported`，停止派发。
```

## `.agents/complexity.md`

```md
# Task Complexity

## Simple

单文件、小配置、只读解释、明显 bug。Manager 可直接处理并自检；`reviewer_for_simple` 开启时交 Reviewer。

## Medium

有限多文件、现有模式内功能、相关测试或文档。Manager -> Builder -> Reviewer。

## Complex

多模块新功能、架构或数据流变化、UI+后端、不明确需求、适合并行探索。Manager 拆分，必要时临时 Agent。

## High Risk

安全、权限、支付、数据删除、迁移、生产配置、外部发布。强制验收，必要时先问用户。
```

## `.agents/execution-loop.md`

```md
# Execution Loop

1. Manager 定义目标、验收标准、验证方式和生命周期阶段。
2. Manager 输出 `管理决策`。
3. Builder 做最小必要变更。
4. Reviewer 对照验收标准检查。
5. Needs Changes 时回到 Builder 或对应生命周期阶段。
6. Pass 后 Manager 汇总。
7. 记录 task log、iteration log、lifecycle loop、必要 decisions / lessons。
```

## `.agents/validation-plan.md`

```md
# Validation Plan

| 任务编号 | 验证方式 | 命令或检查 | 预期结果 | 实际结果 | 备注 |
| --- | --- | --- | --- | --- | --- |

- 优先运行最小相关测试。
- 无法运行时说明原因。
- 高风险任务必须有明确验证证据或人工确认。
```

## `.agents/iteration-log.md`

```md
# Iteration Log

| 任务编号 | 轮次 | Builder 结果 | Reviewer 结论 | 下一步 | 时间 |
| --- | --- | --- | --- | --- | --- |
```

## `.agents/lifecycle-loop.md`

```md
# Lifecycle Loop

| 任务编号 | 当前阶段 | 使用能力 | 结果 | 下一阶段 | 时间 |
| --- | --- | --- | --- | --- | --- |

阶段：spec, plan, build, test, review, webperf, code-simplify, ship。
只记录阶段摘要，不粘贴完整技能输出。
```

## `.agents/history-retention.md`

```md
# History Retention

- `task-log.md`、`iteration-log.md`、`lifecycle-loop.md` 默认各保留最近 50 条。
- `agent-skills.md` 规则长期保留，探测结果只保留最近摘要。
- `session-registry.md` 保留当前任务、活动子智能体和最近 20 条关闭记录。
- 旧记录移动到 `.agents/archive/YYYY-MM.md`。
- `.agents/archive/summary.md` 记录月份、数量、主题、关键风险。
- 默认不读取归档；排查、审计、复盘或用户要求时才读取。
- 归档前脱敏敏感信息。
- 除非用户明确要求，不删除归档。
```

## `.agents/decisions.md`

```md
# Decisions

| ID | Date | Decision | Reason | Alternatives Considered | Impact | Revisit When |
| --- | --- | --- | --- | --- | --- | --- |
```

## `.agents/lessons.md`

```md
# Lessons

| Date | Context | Lesson | Applies To | Avoid Next Time |
| --- | --- | --- | --- | --- |
```

## `.agents/failure-recovery.md`

```md
# Failure Recovery

## Test Failure

记录命令和关键错误；判断是本次变更、环境还是既有问题；本次变更则回 Builder 修复。

## Capability Unavailable

记录限制，使用本地降级方案，必要时更新 capabilities 或 agent-skills。

## Builder Blocked

报告尝试过的步骤；Manager 缩小范围、做小研究或问用户。

## Reviewer Rejects

Manager 将发现转成修复任务，Builder 只修必要范围，Reviewer 复查。

## Destructive Or External Actions

删除数据、破坏性命令、新依赖、生产配置、提交/发布、外发数据前必须问用户。
```
