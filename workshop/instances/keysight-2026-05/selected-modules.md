# 本场选用的 Module 清单

> 顺序按 AGENDA 时间轴排列，而不是按 module 编号。
> 每个 module 都注明了"本场用哪条 demo 路径"，避免现场再翻找。

## 1. `../../modules/01-claude-md/`

- **用在**：Part 2（00:30 – 01:15），项目记忆四层架构
- **本场首选 demo**：公库 `02-Memory/projects/01-web-app/CLAUDE.md`（React + TS + Vite 电商前端样板）
- **对照 demo**：公库 `02-Memory/projects/03-bloated-claude-md/`（630 行反例 + 重构对照版）
- **演示重点**：精简正例 → 反例 → 重构后的对比，让受众看到"好 CLAUDE.md 长什么样"

## 2. `../../modules/03-subagents/`

- **用在**：Part 2 末段（约 01:00 – 01:10），代码库分治
- **本场首选 demo**：公库 `03-SubAgents/projects/02-test-runner/`（含 `package.json` + `src/` + `calculator.test.js`）
- **演示重点**：sub-agent 用 haiku 模型 + 独立上下文，把高噪声测试输出压成 pass/fail 摘要

## 3. `../../modules/02-skills/`

- **用在**：Part 3（01:25 – 02:00），团队 Skill 库
- **本场首选 demo**：公库 `04-Skills/projects/03-financial-skill/`（SKILL.md + reference/ 三层 + scripts + templates）
- **演示重点**：渐进披露——SKILL.md 1.4K tokens、reference 加载 1.5K、scripts 加载 4K，token 计量直观

## 4. `../../modules/07-team-engineering/`

- **用在**：Part 3 末段（约 02:00 – 02:10），团队 plugin 打包
- **本场首选 demo**：公库 `10-Plugins/projects/03-team-toolkit/`（commands + agents + skills + hooks + .mcp.json 一锅出）
- **演示重点**：六种扩展机制（Commands / Agents / Skills / Hooks / MCP / Memory）组合成一个团队工具包

## 5. `../../modules/04-hooks/`

- **用在**：Part 4（02:35 – 02:50），质量门禁与风险控制
- **本场首选 demo**：公库 `06-Hooks/projects/01-safety-hooks/`（block-dangerous.sh + protect-files.sh + audit-log.sh）
- **演示重点**：现场让 Claude Code 尝试 `rm -rf /`，被 PreToolUse hook 当场拦截——视觉冲击最强的 demo

## 没用上的 module

- `../../modules/05-mcp/`：Tech Talk 时长不够，留 Q&A 顺带提
- `../../modules/06-dual-axis-patterns/`：属于 Agent 设计模式主题，与本场 AI 辅助编程主线不重合
