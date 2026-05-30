# Keysight Tech Talk · 时间表与模块映射

> 总时长 2.5 – 3 小时，含 10 分钟休息。每段时间都标注了对应的 module 路径。
> 客户给出的 10 个话题，全部在最后一张"话题覆盖映射表"中逐条对位。

## 时间表

| 时段 | 板块 | 对应 Module | 形式 |
|---|---|---|---|
| 00:00 – 00:30 | Part 1：AI 辅助编程的工程化思维 | 概念串讲，不绑定单一 module | 讲授 |
| 00:30 – 01:15 | Part 2：让 AI 真正读懂你的代码库 | `../../modules/01-claude-md/` + `../../modules/03-subagents/` | 讲授 + 演示 |
| 01:15 – 01:25 | 休息 | — | — |
| 01:25 – 02:10 | Part 3：从 Prompt 到 Skill | `../../modules/02-skills/` + `../../modules/07-team-engineering/` | 讲授 + 演示 |
| 02:10 – 02:55 | Part 4：Token 经济学 | `../../modules/04-hooks/`（质量门禁段） | 讲授 + 演示 |
| 02:55 – 03:00 | 总结 + Q&A | — | 讨论 |

## Part 1 拆解（00:00 – 00:30）

- 00:00 – 00:05 开场 + 自我介绍 + 录制说明
- 00:05 – 00:15 从 Vibe Coding 到 Harness Engineering 的三阶段
- 00:15 – 00:25 Harness 五大组件：Memory / Skills / SubAgents / Hooks / Tools
- 00:25 – 00:30 风险边界：什么场景该 Vibe，什么场景必须工程化

## Part 2 拆解（00:30 – 01:15）

- 00:30 – 00:40 痛点还原（"为什么 AI 记不住我的项目"）
- 00:40 – 00:55 四层记忆架构讲解（CLAUDE.md / Rules / Auto-Memory / Skills）
- 00:55 – 01:10 **现场 Demo**：`../../modules/01-claude-md/` 的精简正例 + 反例对照；再用 `../../modules/03-subagents/` 的 test-runner 演示"只读 SubAgent 分治读大代码库"
- 01:10 – 01:15 中英文语言选择建议

## Part 3 拆解（01:25 – 02:10）

- 01:25 – 01:35 痛点还原（"好 prompt 困在个人经验里"）
- 01:35 – 01:45 SKILL.md 结构 + 三种 Skill 类型 + 渐进披露
- 01:45 – 02:00 **现场 Demo**：`../../modules/02-skills/` 的 financial-skill（三层 reference 加载）
- 02:00 – 02:10 **现场 Demo**：`../../modules/07-team-engineering/` 的 team-toolkit plugin（commands + agents + skills + hooks + mcp 一锅出）

## Part 4 拆解（02:10 – 02:55）

- 02:10 – 02:20 Token 经济学三个概念 + 模型选择矩阵
- 02:20 – 02:35 减少 AI 幻觉的四个手段：结构化 Context / Hooks 校验 / SubAgent 隔离 / 信息注入>信息生成
- 02:35 – 02:50 **现场 Demo**：`../../modules/04-hooks/` 的 safety-hooks（rm -rf 拦截可见效果）
- 02:50 – 02:55 五个立竿见影的成本优化手段清单

## 客户 10 话题 ↔ Module 映射表

| # | 客户原始话题 | 主要落点 | 对应 Module | 深度 |
|---|---|---|---|---|
| 1 | Prompt / Skills / Context 质量 | Part 1 + Part 2 + Part 3 | `01-claude-md` + `02-skills` | 深度 |
| 2 | 减少 AI 幻觉 | Part 4 | `04-hooks` | 深度 |
| 3 | Vibe Coding 工程化 | Part 1 | 不绑定单一 module | 框架 |
| 4 | AI 理解现有 Codebase | Part 2 | `01-claude-md` + `03-subagents` | 深度 + Demo |
| 5 | 上下文与长期记忆 | Part 2 + Part 4 | `01-claude-md` | 深度 |
| 6 | Token 优化与成本控制 | Part 4 | `04-hooks` + `02-skills`（渐进披露段） | 深度 + Demo |
| 7 | Agent 与 Skills 能力复用 | Part 3 | `02-skills` + `03-subagents` | 深度 + Demo |
| 8 | 团队协作与知识共享 | Part 3 末段 | `07-team-engineering` | 方案 + Demo |
| 9 | 最新技术进展 | Part 1 | 八大工具对比段，不绑定 module | 框架 |
| 10 | 语言选择对效果的影响 | Part 2 末段 | `01-claude-md`（顺带） | 实用建议 |

> 10 个话题全覆盖。深度档（带 Demo）= #1 / #2 / #4 / #6 / #7 / #8；框架档 = #3 / #5 / #9；实用建议 = #10。
