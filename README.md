# AI Agent Playbook

这个仓库收集可复制到项目中的 AI 使用技巧、工作流模板和 Agent 协作方法。目标是把零散的 AI 使用经验沉淀成可复用的 Markdown Playbook，方便在不同项目、不同工具和不同工作场景中快速启用。

当前内容包括多 Agent 协作、Superpowers / skills 自动调用、agent-skills 生命周期路由、Ponytail 最小实现/反过度设计门禁，以及 AI 工作上下文系统。后续也可以继续扩展更多 AI 编程、办公、知识管理、自动化和团队协作技巧。

每个目录都是一个独立能力包。进入对应目录后，阅读该目录的 `README.md`，再把对应目录复制到目标项目或让 AI 按需读取其中的 Markdown 文档。

为了减少 token 消耗，多 Agent 能力包采用“短入口 + 按需模板”：日常只读 `multi-agent-bootstrap.md`，初始化或升级时才读取 `templates/`。

各能力包初始化后都会写入自己的版本记录文件。初始版本统一为 `v0.1.0`；后续如果 AI 修改 Prompt 规则、目录结构、协作协议或模板结构，必须自动按 patch / minor / major 规则递增版本号并记录变更。普通业务任务或新增内容资料不递增版本。

各能力包也应支持按需能力发现：当任务可能受益于插件、skill、MCP tool、Superpowers、agent-skills 或项目本地工具时，AI 应自动查询并选择合适能力辅助；如果没有合适能力，则降级为本地流程。能力发现应按需触发，不应为了“用了工具”而扩大任务范围或消耗大量上下文。

## 目录说明

| 目录 | 功能 | 适合场景 | 默认状态 |
| --- | --- | --- | --- |
| `multi-agent-superpowers` | 多 Agent 协作模板，采用短入口 + `templates/` 按需读取，支持自动探测 Superpowers、skills、plugins、MCP tools、Ponytail 和项目本地工具。 | 想要基础 Manager / Builder / Reviewer 流程，并让助手围绕成功标准循环执行、验证、修正，同时避免过度设计。 | 默认关闭，本地可启用 |
| `multi-agent-superpowers-agentskills` | 多 Agent 协作模板，采用短入口 + `templates/` 按需读取，额外集成 `addyosmani/agent-skills` 或兼容技能库，并把 Ponytail 映射到 `/build`、`/review`、`/code-simplify`。 | 想把多 Agent 协作和工程生命周期技能结合起来，适合更规范的需求、计划、实现、测试、审查、发布流程。 | 默认关闭，本地可启用 |
| `work-context-system` | AI 工作上下文系统模板，提供人物库、项目库、事件库、产出模板库的 Markdown 结构和提示词，并加入最小充分上下文原则。 | 想让 AI 更懂“写给谁看”、项目背景、历史事件和固定产出格式，适合汇报、复盘、方案、会议纪要等职场场景，同时控制资料库膨胀。 | 手动初始化 |
| `vibe-coding-preflight` | Vibe Coding 开发前准备模板，先整理项目文件夹、定义 MVP、成功标准、验证计划、执行循环、docs 和 Git，再进入编码。 | 新建 AI 产品、网页、工具或小项目时，想避免一上来就写代码导致项目混乱。 | 手动执行 |

## 通用使用流程

1. 选择一个目录。
2. 打开该目录的 `README.md`。
3. 优先把整个能力目录复制到目标项目，或让 AI 能读取该目录。
4. 在目标项目中打开 AI 编程助手或 Agent 会话。
5. 按目录 README 的说明，让助手先读入口文档，再按需读取模板。
6. 让助手把规则、模板或上下文结构写入目标项目。

## 按需能力发现

所有能力包都遵循同一原则：

- 任务涉及专业领域、复杂判断、文件生成、测试验证、浏览器检查、代码审查、隐私脱敏、文档排版或外部资料查询时，先判断是否有合适插件、skill、MCP tool、Superpowers、agent-skills 或项目本地工具。
- 如果当前工具支持能力发现，优先使用能力发现；如果不支持，根据当前可见工具和项目文件保守判断。
- 只调用和当前任务直接相关的能力，不全量加载技能库。
- 调用前说明使用什么能力、为什么适合、不可用时怎么降级。
- 能力不可用时继续使用本地流程，并在结果中说明限制。

## Ponytail / 最小化能力

如果当前环境存在 [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) 或类似最小实现、YAGNI、代码简化、输出压缩能力，各目录会按自己的目标使用：

- `multi-agent-superpowers`：作为 Builder 写代码前和 Reviewer 审查时的最小实现门禁。
- `multi-agent-superpowers-agentskills`：结合 agent-skills 生命周期，重点作用于 `/build`、`/review`、`/code-simplify`。
- `work-context-system`：转换为“最小充分上下文”原则，减少重复记录、无用背景和产出膨胀。

检测不到 Ponytail 时，不影响使用；对应目录会使用本地 Markdown 规则降级。

如果选择 `work-context-system`，则按该目录 README 初始化 `.work-context/`，它不依赖 `.agents/` 或多 Agent 开关。

如果选择 `vibe-coding-preflight`，则按该目录 README 初始化 `docs/` 和项目 README，它适合作为新项目正式编码前的第一步。

## 开关机制

模板默认创建项目级设置：

```text
.agents/settings.md
#### 项目默认不启用多 Agent，适合提交到团队仓库
multi_agent_default: off

#### 简单任务默认不启用 Reviewer
reviewer_for_simple_default: off

#### 默认不预授权真实 sub-agent；需要本地开启后才可自动创建 Builder / Reviewer
real_subagents_default: off

#### 默认自动使用 Ponytail 或本地最小化门禁
ponytail_default: auto
```

个人本地启用：

```text
.agents/local-settings.md
#### 本地启用多 Agent
multi_agent: on

#### 本地启用简单任务 Reviewer 复核
reviewer_for_simple: on

#### 本地授权真实 sub-agent 自动创建
real_subagents: on

#### 本地启用 Ponytail / 最小化门禁
ponytail: auto
```

其中 `reviewer_for_simple: on` 表示简单任务也会启用 Reviewer 复核；不需要时可以省略或设为 `off`。
其中 `real_subagents: on` 表示如果客户端开放 `spawn_agent` / `sub-agent` 工具，Manager 可以自动创建真实 Builder / Reviewer 子智能体。

启用本地设置时，助手应自动把它加入目标项目的 `.gitignore`：

```gitignore
.agents/local-settings.md
.agents/archive/
```

如果目标项目没有 `.gitignore`，助手应创建一个并写入这些规则。这样可以把多 Agent 协作模板提交到仓库，但不会强制所有同事启用，也不会把本地运行归档提交进去。

## 版本记录文件

| 能力包 | 目标项目中的版本文件 | 初始版本 | 何时递增 |
| --- | --- | --- | --- |
| `multi-agent-superpowers` | `.agents/prompt-version.md` | `v0.1.0` | 修改多 Agent 规则、`.agents/` 协作规则或 `AGENTS.md` |
| `multi-agent-superpowers-agentskills` | `.agents/prompt-version.md` | `v0.1.0` | 修改多 Agent 规则、agent-skills 路由、`.agents/` 协作规则或 `AGENTS.md` |
| `work-context-system` | `.work-context/version.md` | `v0.1.0` | 修改上下文系统规则、目录结构、隐私规则、最小充分上下文规则或模板结构 |
| `vibe-coding-preflight` | `docs/prompt-version.md` | `v0.1.0` | 修改 Preflight 规则、`docs/` 准备结构或 README 入口 |
