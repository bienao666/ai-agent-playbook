# Capability Rules Template

Use this file only during initialization or compatible upgrade.

## `.agents/capabilities.md`

```md
# Capability Registry

## Detected Capabilities

| Name | Type | Best Used By | Use Cases | Notes |
| --- | --- | --- | --- | --- |

## Policy

- 非平凡任务开始前，轻量检查 Superpowers、agent-skills、skills、plugins、MCP tools、Ponytail、本地脚本。
- 只调用能改善正确性、速度、安全、审查质量或产物质量的能力。
- 调用前说明能力、原因、降级方案。
- 不全量加载技能库，不为了用工具扩大范围。

## Routing

- Manager：规划、扫描、任务路由、thread/subagent 协调。
- Builder：实现、重构、生成、浏览器/UI 验证。
- Reviewer：审查、测试、安全、回归、风险。
- QA：测试执行、浏览器验证、截图、回归。
- Docs：文档、格式化、图表、引用。
```

## Superpowers / Skills / Plugins / MCP

- 当前环境存在匹配能力时，按其说明调用。
- 项目 `AGENTS.md` 和用户当前指令优先级高于外部能力偏好。
- 能力不可用时输出一次降级说明，继续本地流程。

Required assignment fields when capability is used:

```md
能力:
原因:
降级方案:
```

## `.agents/ponytail.md`

```md
# Ponytail Minimal Implementation

## Purpose

Ponytail 是可选最小实现门禁，防止过度设计、无必要依赖、无关重构和上下文膨胀。它不替代 Manager / Builder / Reviewer，也不替代 agent-skills。

## Enable

- `ponytail: off` 或 `ponytail_default: off`：关闭。
- `auto`：实现、重构、依赖、架构、审查、代码简化任务启用。
- `strict`：Builder 写代码前必须完成 Ladder。

## Agent Skills Mapping

- `/spec`：挑战需求必要性。
- `/plan`：删除无必要任务和提前设计。
- `/build`：先 Ladder，再最小可验证实现。
- `/review`：检查过度设计和无必要依赖。
- `/code-simplify`：优先删除、合并、复用。

## Ladder

1. 是否真的需要改？
2. 项目里是否已有能力？
3. 标准库、框架或平台能否解决？
4. 已安装依赖能否解决，能否避免新增依赖？
5. 是否能更小改动完成？
6. 最小可验证改动是什么？
7. 需要哪一个最小验证？

## Builder Rules

- 复用现有代码和模式。
- 不为未来假设提前设计。
- 不新增依赖，除非收益明确且现有能力不足。
- 不新增抽象，除非立即减少真实重复或复杂度。
- 更复杂方案必须说明为什么更小方案不够。

## Reviewer Rules

- 检查不必要文件、依赖、抽象、配置、流程。
- 检查是否可用已有代码或框架能力替代。
- 检查是否存在无关重构或提前设计。
- 检查测试、日志、文档是否有验证价值。

## Output Fields

Ponytail:
Ponytail Reason:
Ponytail Fallback:
Ponytail Check:
最小实现取舍:
Ponytail Review:
过度设计风险:
```
