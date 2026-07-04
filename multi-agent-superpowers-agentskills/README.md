# Multi-Agent Superpowers + Agent Skills Bootstrap

这个目录提供一套可复制到任意项目 AI 编程助手或 Agent 会话中的多 Agent 启动文档：

- `multi-agent-bootstrap.md`
- `templates/`

它的作用是让 AI 编程助手在目标项目中初始化一套 Manager / Builder / Reviewer 协作机制，并自动创建 `.agents/`、`AGENTS.md`、任务日志、能力注册、复杂度分级、失败恢复和验收流程。

这个版本额外集成 [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) 或兼容技能库。如果用户环境中存在 agent-skills，助手会自动把它作为工程流程技能层来使用。

该机制默认关闭，适合提交到项目仓库中作为可选协作能力。团队其他成员不会被强制启用；需要使用的人可以在本地开启。

新版采用“短入口 + 按需模板”的结构：日常只需要读取 `multi-agent-bootstrap.md`；初始化或升级时才读取 `templates/` 中的细节模板，从而减少 token 消耗并加快处理速度。

## 使用方式

推荐方式：把整个 `multi-agent-superpowers-agentskills/` 目录复制到目标项目，或让 AI 能访问这个目录。然后在目标项目中打开一个新的 AI 编程助手或 Agent 会话，发送：

```text
请读取 multi-agent-superpowers-agentskills/multi-agent-bootstrap.md 并执行。
初始化或升级 .agents/ 和 AGENTS.md 时，按需读取 multi-agent-superpowers-agentskills/templates/ 下的模板；日常任务不要全量读取 templates。
```

如果当前环境无法读取目录，也可以先粘贴 `multi-agent-bootstrap.md`。AI 会创建最小可用规则，并提示你补充需要的模板文件。

AI 编程助手会自动：

- 扫描当前项目结构和技术栈。
- 创建或更新 `AGENTS.md`。
- 创建或更新 `.agents/` 协作目录。
- 初始化 Manager、Builder、Reviewer 三个基础 Agent。
- 建立任务协议、任务日志、验收清单和失败恢复流程。
- 建立执行闭环、验证计划、迭代记录和 agent-skills 生命周期闭环。
- 探测当前环境可用的 Superpowers、agent-skills、skills、plugins、MCP tools 和项目本地工具。
- 如果存在 agent-skills，自动按工程生命周期调用相关能力。
- 在任务需要时自动查询合适的插件、skill、MCP tool、Superpowers、agent-skills 或项目本地工具辅助。
- 如果环境存在 Ponytail、agent-skills 代码简化能力或类似最小实现能力，会自动作为 `/build` 前和 `/review`、`/code-simplify` 阶段的反过度设计门禁；不存在时使用本地最小实现规则降级。
- 创建 `.agents/settings.md`，默认 `multi_agent_default: off`、`real_subagents_default: off`。
- 创建 `.agents/prompt-version.md`，初始版本为 `v0.1.0`，后续规则变动自动递增版本号。
- 创建 `.agents/session-registry.md`，用于在启用多 Agent 后记录 Builder / Reviewer 等独立会话。
- 创建 `.agents/execution-loop.md`、`.agents/validation-plan.md`、`.agents/iteration-log.md`、`.agents/lifecycle-loop.md`，让任务按生命周期循环推进。
- 创建 `.agents/history-retention.md` 和 `.agents/archive/`，用于控制历史记录膨胀。
- 后续只有在用户本地启用后，才按 Manager 分配、Builder 执行、Reviewer 验收、Manager 汇总的流程工作。

## Token 消耗优化

新版默认采用“摘要优先、按需展开”的方式运行：

- `multi-agent-bootstrap.md` 是短入口，详细模板放在 `templates/`。
- 初始化或兼容升级时才读取模板全文，不影响功能。
- 日常任务只读取开关、任务日志、相关规则和当前生命周期阶段，不每次全量读取 `.agents/`。
- agent-skills 只加载当前阶段匹配的 skill 或 slash command，不全量读取技能库。
- 日常任务不读取 `templates/`，除非要升级规则或修复缺失模板。
- 每次交互先输出精简中文 `管理决策`，明确任务编号、目标、分配对象、原因、验收标准和下一步。
- 简单任务默认由 Manager 精简处理和自检，只输出精简 `管理决策`，不输出完整三段式报告。
- Builder / Reviewer / sub-agent 只接收最小必要上下文。
- 复杂任务、高风险任务、Reviewer 未通过或用户要求详细记录时，才进入完整上下文模式。

## 历史记录保留

历史记录采用“热记录 + 归档摘要”策略：

- `task-log.md` 默认保留最近 50 条。
- `iteration-log.md` 默认保留最近 50 条。
- `lifecycle-loop.md` 中的规则长期保留，阶段变更记录默认保留最近 50 条。
- `session-registry.md` 默认保留当前任务、活动子智能体和最近 20 条关闭记录。
- 旧记录移动到 `.agents/archive/`，按月份归档。
- `.agents/archive/summary.md` 保存归档摘要。
- 默认只读取热记录；排查历史问题、审计、复盘或用户要求时才读取归档。
- 除非用户明确要求，不删除归档文件。

## 按需能力发现

任务涉及需求澄清、计划拆解、实现、测试、审查、安全、性能、浏览器验证、文档生成、图片/表格/PDF 处理或其他专业能力时，Manager 会自动查询合适的插件、skill、MCP tool、Superpowers、agent-skills 或项目本地工具。

查询原则：

- agent-skills 只查当前生命周期阶段匹配的能力。
- 只查当前任务相关能力，不全量加载技能库。
- 找到合适能力时说明能力名称、使用原因和降级方案。
- 没有合适能力时继续使用本地多 Agent 流程。
- 不为了调用能力而扩大任务范围。

## Ponytail 最小实现

这个版本支持集成 [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) 或类似“最小实现 / YAGNI / 代码简化”能力，并会优先和 agent-skills 生命周期结合。

默认配置为：

```text
#### 是否启用 Ponytail 最小实现门禁
ponytail_default: auto
```

含义：

- `auto`：实现、重构、依赖引入、架构改动和审查任务中自动使用；检测不到 Ponytail 时使用本地 Ponytail Ladder。
- `off`：关闭该门禁。
- `strict`：Builder 写代码前必须完成最小实现判断，并在报告中说明取舍。

和 agent-skills 的关系：

- `/spec`：挑战需求是否必须做。
- `/plan`：删掉无必要任务和提前设计。
- `/build`：先走最小实现判断，再写代码。
- `/review`：检查过度设计和无必要依赖。
- `/code-simplify`：优先删除、合并、复用，再考虑新增抽象。

它不会替代 Manager / Builder / Reviewer，也不会替代 agent-skills；它只负责让实现更小、更贴近现有项目。

## 版本号规则

初始化后会生成 `.agents/prompt-version.md`：

- 初始版本：`v0.1.0`
- 文案或说明补充：递增 patch，例如 `v0.1.1`
- 新增可选能力、配置项、兼容规则或 agent-skills 路由：递增 minor，例如 `v0.2.0`
- 改变默认行为、删除能力或改变协议结构：递增 major，例如 `v1.0.0`

只有修改多 Agent Prompt 规则、`.agents/` 协作规则、agent-skills 路由或 `AGENTS.md` 时才递增版本；普通业务开发任务不递增。

## 从旧版本升级

如果你的项目以前已经使用过旧版 `multi-agent-bootstrap.md`，不要手动删除旧的 `.agents/` 或 `AGENTS.md`。

在目标项目中打开 AI 编程助手，然后发送：

```text
请读取下面这份新版 Multi-Agent Bootstrap 文档，并对当前项目已有的 .agents/ 和 AGENTS.md 做兼容升级。
要求：
1. 不覆盖用户已有内容。
2. 不删除旧任务日志、决策记录、经验记录。
3. 只补齐缺失文件、缺失字段和新增规则。
4. 如果同一规则已存在，只更新为新版语义，不重复追加。
5. 升级完成后输出升级摘要。
```

然后让 AI 读取新版 `multi-agent-bootstrap.md` 和 `templates/`；如果只能粘贴文本，先粘贴入口文件，再按 AI 提示补充对应模板。

升级时重点检查：

- `.agents/settings.md` 是否包含带注释的 `multi_agent_default`、`reviewer_for_simple_default` 和 `real_subagents_default`。
- `.agents/local-settings.md` 和 `.agents/archive/` 是否已加入 `.gitignore`。
- `.agents/session-registry.md` 是否存在。
- `.agents/agent-skills.md` 是否存在并包含 lifecycle routing。
- `.agents/ponytail.md` 是否存在，`settings.md` 是否包含 `ponytail_default`。
- 协议字段是否包含 `目标会话` 和 `来源会话`。
- 是否支持客户端原生 `sub-agent`、`spawn_agent`、`worker`、`explorer`。
- 是否支持 `reviewer_for_simple`，让简单任务也可选进入 Reviewer。
- Manager / Builder / Reviewer 输出字段是否已中文化。
- agent-skills 路由是否包含 `/spec`、`/plan`、`/build`、`/test`、`/review`、`/webperf`、`/code-simplify`、`/ship`。
- Ponytail 是否已映射到 `/build`、`/review` 和 `/code-simplify`。
- 是否包含 Token Budget Policy，避免每次任务全量读取 `.agents/` 或 agent-skills 技能库。
- 是否支持短入口 + `templates/` 按需读取，避免每次任务全量读取 Bootstrap 长文档。
- 是否包含 `.agents/prompt-version.md`，并能在后续规则变动时自动递增版本号。

建议让 AI 最后给出：

```md
## Upgrade Summary

已新增:
已更新:
保持不变:
需要人工确认:
```

## 初始化后怎么使用

初始化完成后，多 Agent 协作默认关闭。你可以把这些规则提交到项目仓库，让团队共享，但每个人可以选择是否启用。

本地启用方式：

```text
请在当前项目为我本地启用 Multi-Agent Mode。
创建 .agents/local-settings.md，写入 multi_agent: on，并自动把 .agents/local-settings.md 和 .agents/archive/ 添加到项目根目录 .gitignore。
```

或者你也可以手动创建：

```text
.agents/local-settings.md
```

内容：

```text
multi_agent: on
```

如果希望简单任务也由 Reviewer 复核，可以写成：

```text
#### 本地启用多 Agent 协作模式
multi_agent: on

#### 简单任务也启用 Reviewer 复核
reviewer_for_simple: on

#### 明确授权自动创建真实 sub-agent
real_subagents: on
```

启用时助手应自动把它加入 `.gitignore`：

```gitignore
.agents/local-settings.md
.agents/archive/
```

启用后，在同一个项目里不需要再重复说“按多 Agent 流程”或“按 Manager / Builder / Reviewer 流程”。

如果当前 AI 工具支持 `sub-agent`、`spawn_agent`、`worker`、`explorer` 等原生子智能体工具，Manager 会按任务创建真实子智能体；这类客户端可能会在界面中展示子智能体列表和各自的改动统计。  
如果只支持 thread 或独立会话，Manager 会按任务创建 Builder / Reviewer 独立会话；如果都不支持，则在当前会话中模拟角色切换。

注意：很多客户端不会在子智能体完成后自动从右侧列表移除。新版规则不复用子智能体；创建真实 sub-agent 后必须记录 ID，名称必须带角色和任务号，例如 `Builder-T-001`、`Reviewer-T-001`。`wait_agent` 收到结果后立即执行 `close_agent` 或等效关闭动作。每次任务开始和最终回复前，也会检查历史遗留且当前不用的 sub-agent，并尽量关闭。如果 `close_agent` 返回 `not found`，记录为 `Closed/Not Found` 即可。

你可以直接像平常一样提需求：

```text
修复登录页按钮点击无响应的问题
```

或者：

```text
给订单列表增加导出 CSV 的功能
```

AI 编程助手应自动：

- 检查 Multi-Agent Mode 是否已启用。
- 把请求交给 Manager intake。
- 判断任务复杂度。
- 判断是否需要 Builder、Reviewer 或临时 Agent。
- 判断是否有 Superpowers、agent-skills、skills、plugins 或 MCP tools 可以使用。
- 如果有 agent-skills，按任务阶段自动匹配 `/spec`、`/plan`、`/build`、`/test`、`/review`、`/webperf`、`/code-simplify` 或 `/ship`。
- 按合适流程执行、验收并汇总。
- 如果 Reviewer 未通过，Manager 会让流程回到 `/build`、`/test` 或必要时 `/plan`，而不是一次失败就结束。

如果当前环境安装了 Superpowers 或其他 skills，可以这样说：

```text
这个任务可以优先使用 Superpowers 里的相关能力。
```

这不是必需提示，只是当你想特别强调使用 Superpowers 时可以加上。

如果当前环境安装了 agent-skills，也不需要每次显式提示。助手应自动识别任务阶段：

- 需求不清或需要定义功能：`/spec`
- 需要拆解计划：`/plan`
- 需要实现：`/build`
- 需要测试或调试：`/test`
- 需要审查：`/review`
- 需要性能审计：`/webperf`
- 需要简化代码：`/code-simplify`
- 需要发布准备：`/ship`

以上自动识别仅在 Multi-Agent Mode 启用后作为协作调度的一部分运行。未启用时，助手仍可按普通单 Agent 方式使用当前环境里的工具和技能。

## 建议确认项

第一次粘贴文档后，可以让 AI 编程助手确认：

```text
请确认你已经创建或更新 AGENTS.md 和 .agents/*，并说明之后你会如何按 Manager / Builder / Reviewer 流程工作。
```

确认通过后，这个项目就具备了项目级多 Agent 协作规则。

## 开关规则

默认关闭：

```text
.agents/settings.md
#### 项目默认不启用多 Agent，适合提交到团队仓库
multi_agent_default: off

#### 简单任务默认不启用 Reviewer
reviewer_for_simple_default: off

#### 默认不预授权真实 sub-agent；需要本地开启后才可自动创建 Builder / Reviewer
real_subagents_default: off

#### 默认自动使用 Ponytail 或本地最小实现门禁
ponytail_default: auto
```

个人本地开启：

```text
.agents/local-settings.md
#### 本地启用多 Agent
multi_agent: on

#### 本地启用简单任务 Reviewer
reviewer_for_simple: on

#### 本地授权真实 sub-agent 自动创建
real_subagents: on

#### 本地启用 Ponytail 最小实现门禁
ponytail: auto
```

个人本地关闭：

```text
.agents/local-settings.md
#### 本地关闭多 Agent
multi_agent: off

#### 本地关闭简单任务 Reviewer
reviewer_for_simple: off

#### 本地关闭真实 sub-agent 自动创建
real_subagents: off

#### 本地关闭 Ponytail 最小实现门禁
ponytail: off
```

当前消息也可以临时控制：

```text
这次启用多 Agent 处理。
```

或：

```text
这次不要使用多 Agent，直接处理。
```

## 注意事项

- 直接把文档喂给当前 AI 编程助手，只能保证当前会话知道这些规则。
- 为了降低 token，推荐复制整个目录，让 AI 按需读取 `templates/`；不要每次把所有模板全文粘贴进对话。
- 让 AI 编程助手把规则写入 `AGENTS.md` 和 `.agents/` 后，新会话更容易自动继承这些规则。
- 初始化完成后，多 Agent 流程默认关闭，避免影响不想启用的同事。
- 启用后正常提需求即可，不需要重复提示助手使用该流程。
- 如果目标项目已经有 `AGENTS.md`，助手应保留原内容，只追加缺失的多 Agent 规则。
- 如果当前环境不支持真实 thread、subagent 或跨会话消息，助手会在同一会话中用明确角色切换模拟多 Agent 协作。
- 如果当前环境没有 Superpowers 或类似增强能力，助手会自动降级为普通本地多 Agent 流程。
- 如果当前环境没有 agent-skills，助手会跳过 agent-skills 生命周期路由，不影响正常使用。
