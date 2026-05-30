# Keysight Technologies · AI 辅助编程工程化实战 Tech Talk

> 本目录是 workshop 在**是德科技**线上 Tech Talk 的一次落地实例。
> 模板说明：`../README.md`；可复用模块：`../../modules/`。

## 客户背景

是德科技（Keysight Technologies）是电子测量与测试设备领域的全球头部供应商，研发团队规模大、工程文化扎实，对工具的"可控性、可审计、可团队复用"有天然要求。本次 Tech Talk 通过易势腾（合作方）对接，受众是已经在用 AI 编程工具但希望从"能用"走到"用好"的研发工程师。

## 培训信息

| 项 | 内容 |
|---|---|
| 客户 | Keysight Technologies（是德科技） |
| 形式 | 线上 Tech Talk（Zoom / WebEx） |
| 时长 | 2.5 – 3 小时（含一次 10 分钟休息） |
| 日期 | 2026 年 6 月初（周五） |
| 讲师 | 黄佳（咖哥） |
| 语言 | 中文为主，代码与命令保留英文 |
| 录制 | 由客户侧统一录制，内部回放 |

## 受众画像

- 一线研发工程师，部分中层技术负责人
- 已经在日常工作里使用 AI 编程工具（Copilot / Cursor / Claude Code 等），不属于"零基础"
- 主流语言为 Python、C/C++、少量 TypeScript，业务上偏仪器固件、测试自动化、上位机软件
- 对"AI 写出来的代码能否进生产代码库"持谨慎态度，希望听到工程化方法而不是 demo 炫技

## 受众真实痛点（来自客户前期访谈的 10 个话题）

1. Prompt / Skills / Context 质量怎么写才稳定
2. 减少 AI 幻觉的系统性方法
3. Vibe Coding 怎么走向工程化
4. AI 如何真正读懂现有的大型 Codebase
5. 上下文与长期记忆怎么管理
6. Token 优化与成本控制
7. Agent 与 Skills 的能力复用
8. 团队协作与知识共享
9. AI 编程工具的最新技术进展
10. 中英文语言选择对效果的影响

> 这 10 个话题在 `AGENDA.md` 里逐条映射到具体模块和讲解位置。

## 这次用了哪些 modules

| 顺序 | Module | 在本场的作用 |
|---|---|---|
| 1 | `../../modules/01-claude-md/` | Part 2 的核心——项目记忆与 Codebase 理解 |
| 2 | `../../modules/02-skills/` | Part 3 的核心——把 Prompt 升级为团队能力资产 |
| 3 | `../../modules/03-subagents/` | Part 2 的代码库分治 + Part 3 的 Bug 修复流水线 |
| 4 | `../../modules/04-hooks/` | Part 1 的风险边界 + Part 4 的质量门禁 |
| 5 | `../../modules/07-team-engineering/` | Part 3 末尾的团队 Plugin 打包演示 |

未上场的模块：

- `../../modules/05-mcp/`：Tech Talk 时长有限，MCP 留到 Q&A 顺带提一句，不做正式 demo
- `../../modules/06-dual-axis-patterns/`：这是 Agent 设计模式的内容，与本场"AI 辅助编程"主线不重合，不引入

## 配套素材位置

- PPT 文件：`../ppt/keysight-2026-06.pptx`（待补，从私库 `60-企业培训/62-是德科技培训/` 同步）
- 大纲原稿（私库）：`60-企业培训/62-是德科技培训/培训大纲-是德科技-AI辅助编程工程化实战.md`
- 现场截图：`screenshots/`（培训当天补齐）

## 后续复盘指引

- 培训结束后 24h 内：把现场提问（特别是关于仪器固件、测试自动化场景的问题）回填到本 README 的"受众真实痛点"段，作为后续同行业培训的输入
- 把客户反馈里能脱敏的部分写进 `../../README.md` 的"Workshop 应用案例"段
