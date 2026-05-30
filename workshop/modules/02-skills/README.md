# Module 02 · Skills 渐进披露

## 这个模块解决什么问题

「让 Claude 会写财报分析」「让 Claude 会做安全审计」——能力一多，全塞进 CLAUDE.md 就臃肿到爆，每次会话付一堆 token 即使根本用不到这些能力。Skills 解决的就是这件事：把能力包做成**按需加载**的模块，触发词命中才把完整内容拉进来。SKILL.md 就是那个**入口路由器**，决定要不要进入这个能力包。

## 学完你会

1. 看得懂 SKILL.md frontmatter 三个字段（name / description / allowed-tools）各自起什么作用，写 description 的时候知道怎么让触发条件准确
2. 会用「三层架构」省 token：SKILL.md 总入口（1-2 KB）→ reference/ 详细说明（按需加载）→ scripts/ 可执行脚本（真正调用时才跑）
3. 知道什么任务该做成 skill、什么任务该写在 CLAUDE.md、什么任务该开 sub-agent，三者职责边界清楚

## Demo 怎么跑

公库已有完整 skill 样本，直接引用：

```bash
# 1. 进到 demo 目录
cd ../../../04-Skills/projects/03-financial-skill/

# 2. 看 skill 的三层结构
ls -la
#   SKILL.md                          ← 入口路由（1.4 KB）
#   reference/revenue.md              ← 详细说明，按需加载
#   reference/costs.md
#   reference/profitability.md
#   templates/analysis_report.md      ← 报告模板
#   scripts/calculate_ratios.py       ← 可执行计算

# 3. 把 skill 部署到当前用户的 skill 目录
mkdir -p ~/.claude/skills/financial-analyzing/
cp -r * ~/.claude/skills/financial-analyzing/

# 4. 启动 Claude，触发 skill
claude
# 在 Claude 里说：
#   > analyze the revenue growth in attached financial-statement.csv

# 5. 观察 Claude 怎么按需加载 reference/revenue.md 而不是把三份 reference 都塞进 context
```

详细 demo 引用说明见 [demo/README.md](demo/README.md)。

## 底层原理

SKILL.md frontmatter 的关键字段：

```yaml
---
name: financial-analyzing
description: Use when the user asks to analyze financial statements, revenue, costs, profitability, or asks for financial ratio computation.
allowed-tools:
  - Read
  - Bash
  - Glob
---
```

启动会话时，Claude Code 把所有可用 skills 的 `name + description` 列出来当作「能力清单」一并塞进 system context。**只塞清单，不塞内容**——这就是 progressive disclosure 的核心：清单很短（几十个 KB 撑得起几十个 skill），内容很长但按需加载。

触发流程：
1. 用户提问 → Claude 看清单 → 判断 description 是否匹配
2. 匹配 → 加载 SKILL.md 完整内容到 context
3. SKILL.md 引用了 reference/revenue.md → Claude 用 Read 工具按需读
4. SKILL.md 引用了 scripts/calculate_ratios.py → Claude 用 Bash 工具调用

Token 经济学：
- 不触发的 skill：只付 description 那一行的 token
- 触发的 skill：付 SKILL.md 的 token（~1500），reference 文件按需付费
- 对比把所有能力写进 CLAUDE.md：每次会话都付全部内容的 token

## 在 Claude Code 中怎么落地

- 公库目录：[`04-Skills/`](../../../04-Skills/)
- Skill 安装位置：
  - 用户级：`~/.claude/skills/<skill-name>/SKILL.md`
  - 项目级：`<project>/.claude/skills/<skill-name>/SKILL.md`
- 触发验证：在 Claude Code 里问 `/skills`，能看到所有可用 skill 清单
- 写 description 的窍门：用「Use when ...」开头 + 列具体触发场景，别用「This skill helps with ...」这种自指句

## 对应客户 PDF 话题

每场 instance 在 `instances/<client>/AGENDA.md` 里维护映射。常见对应话题：

- 「能不能让 Claude 学会我们公司的内部规范？」→ 本 module（做成 internal-standard skill）
- 「不同业务线想用不同能力但不想互相干扰」→ 本 module（一业务一 skill，互相隔离）
