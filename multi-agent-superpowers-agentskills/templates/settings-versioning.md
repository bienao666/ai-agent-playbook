# Settings And Versioning Template

Use this file only during initialization or compatible upgrade.

## `.agents/settings.md`

```md
# Multi-Agent Settings

#### 是否默认启用多 Agent 协作模式。
#### off：只安装规则，不自动启用，适合提交到团队仓库。
#### on：项目默认启用，多数团队不建议直接提交为 on。
multi_agent_default: off

#### 简单任务是否也需要 Reviewer 复核。
#### off：简单任务由 Manager 自检即可。
#### on：简单任务也进入 Manager + Reviewer 流程。
reviewer_for_simple_default: off

#### 是否在项目级默认授权真实 sub-agent / spawn_agent。
#### off：默认不自动创建真实 sub-agent。
#### on：客户端开放工具时可自动创建 Builder / Reviewer。
real_subagents_default: off

#### 是否启用 Ponytail 最小实现门禁。
#### auto：检测到 Ponytail 或 agent-skills 简化能力时调用，否则使用本地门禁。
#### off：关闭。
#### strict：实现前必须完成 Ponytail Ladder。
ponytail_default: auto

## Local Override

个人本地可创建 `.agents/local-settings.md`：

    #### 本地启用多 Agent
    multi_agent: on

    #### 简单任务也启用 Reviewer
    reviewer_for_simple: on

    #### 本地授权真实 sub-agent 自动创建
    real_subagents: on

    #### 本地启用 Ponytail
    ponytail: auto

本地关闭示例：

    #### 本地关闭多 Agent
    multi_agent: off

    #### 本地关闭简单任务 Reviewer
    reviewer_for_simple: off

    #### 本地关闭真实 sub-agent 自动创建
    real_subagents: off

    #### 本地关闭 Ponytail
    ponytail: off
```

## `.gitignore`

Ensure root `.gitignore` contains:

```gitignore
.agents/local-settings.md
.agents/archive/
```

Append without duplicates. Create `.gitignore` if missing.

## `.agents/prompt-version.md`

```md
# Prompt Version

Current Version: v0.1.0

## Versioning Rules

- patch：文案、说明、错别字、示例补充，不改变行为。
- minor：新增可选能力、配置项、检查项、兼容规则、agent-skills 路由，不破坏旧行为。
- major：改变默认行为、删除能力、改变文件结构或协议字段含义。

## Changelog

| Version | Date | Change Type | Summary | Requires Manual Review |
| --- | --- | --- | --- | --- |
| v0.1.0 | YYYY-MM-DD | initial | 初始化 Multi-Agent + Agent Skills Bootstrap 规则。 | No |
```

Upgrade rule:

- 已存在时不要覆盖历史。
- 多 Agent 规则、agent-skills 路由、`.agents/` 协作规则或 `AGENTS.md` 变化才递增版本。
- 普通业务任务不递增。
