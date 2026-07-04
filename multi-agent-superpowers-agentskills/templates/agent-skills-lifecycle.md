# Agent Skills Lifecycle Template

Use this file only during initialization or compatible upgrade.

## `.agents/agent-skills.md`

```md
# Agent Skills Integration

## Detection

Check whether the environment exposes `addyosmani/agent-skills`, compatible skills, or slash commands:

- `/spec`
- `/plan`
- `/build`
- `/test`
- `/review`
- `/webperf`
- `/code-simplify`
- `/ship`

If detected, record available commands and skills in `.agents/capabilities.md`.

## Lifecycle Routing

| Situation | Preferred agent-skills | Role |
| --- | --- | --- |
| 需求不清 | `interview-me`, `idea-refine`, `/spec` | Manager |
| 定义功能 | `spec-driven-development`, `/spec` | Manager |
| 拆解任务 | `planning-and-task-breakdown`, `/plan` | Manager |
| 实现 | `incremental-implementation`, `test-driven-development`, `/build` | Builder |
| 测试/调试 | `browser-testing-with-devtools`, `debugging-and-error-recovery`, `/test` | QA / Reviewer |
| 审查 | `code-review-and-quality`, `/review` | Reviewer |
| 安全 | `security-and-hardening` | Reviewer |
| 性能 | `performance-optimization`, `/webperf` | Reviewer / QA |
| 简化代码 | `code-simplification`, `/code-simplify` | Reviewer / Builder |
| 发布 | `shipping-and-launch`, `/ship` | Manager |

## Rules

- 只加载当前生命周期阶段匹配的 skill 或 slash command。
- 不全量读取 agent-skills 库。
- 调用前说明使用的 skill、原因、降级方案。
- agent-skills 不可用时，只记录一次降级结论，继续本地多 Agent 流程。
- 用户当前指令和项目本地规则优先于外部技能偏好。
```
