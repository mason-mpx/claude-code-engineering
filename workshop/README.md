# Claude Code Workshop · 工程化与设计模式

一套可 fork 的企业培训资产包。把 Claude Code 五大扩展机制（Memory / Skills / Sub-Agents / Hooks / MCP）和 Agent 设计模式之美的双轴框架打包成 7 个独立模块，每个模块自带 slides + 讲师脚本 + demo 引用 + 时长建议，方便按客户话题挑组装。

不是一份成品 PPT。是一套**积木**——每一家企业的培训都是一次新组合。

## 全景：5 大扩展机制 + 双轴设计模式

```
                        ┌─────────────────────────────────────────────┐
                        │           Claude Code Workshop              │
                        └──────────────────┬──────────────────────────┘
                                           │
        ┌──────────────────────────────────┼──────────────────────────────────┐
        │                                  │                                  │
   ┌────▼─────┐   ┌──────────┐   ┌────────▼─────┐   ┌──────────┐   ┌────────▼─────┐
   │  Memory  │   │  Skills  │   │  Sub-Agents  │   │  Hooks   │   │     MCP      │
   │CLAUDE.md │   │ SKILL.md │   │ delegation   │   │event auto│   │external tool │
   │  M01     │   │   M02    │   │     M03      │   │   M04    │   │     M05      │
   └────┬─────┘   └────┬─────┘   └──────┬───────┘   └────┬─────┘   └──────┬───────┘
        │              │                │                │                │
        └──────────────┴────────────────┴────────────────┴────────────────┘
                                          │
                              ┌───────────▼──────────┐
                              │  Dual-Axis Patterns  │
                              │   7 capabilities ×   │
                              │   6 topologies = 28  │
                              │         M06          │
                              └───────────┬──────────┘
                                          │
                              ┌───────────▼──────────┐
                              │  Team Engineering    │
                              │  Plugins · 团队工具包 │
                              │         M07          │
                              └──────────────────────┘
```

底层逻辑：M01-M05 是 Claude Code 提供的**扩展点**；M06 是怎么**用这些扩展点搭出 agent**的设计语言；M07 是**怎么把整套工程化交付给团队**。

## How to use

### 学员视角（跟一场培训）

```bash
git clone https://github.com/huangjia2019/claude-code-engineering
cd claude-code-engineering/workshop/modules/01-claude-md
# 跟着 module README 走，demo/ 里有跑通指令
```

7 个 module 互相独立。哪个先讲哪个后讲取决于讲师的 AGENDA。学员可以按顺序读，也可以挑模块复习。

### 讲师视角（准备一场培训）

```bash
# 1. 先看 PROCESS.md：怎么 fork 一份本次客户专属版本
cat workshop/PROCESS.md

# 2. 看 instances/ 下历史交付，找跟你这家客户最像的一场
ls workshop/instances/

# 3. 复制一份开干
cp -r workshop/instances/<closest-match>/ workshop/instances/<your-client>-<yyyy-mm>/
```

详细 fork 流程见 [PROCESS.md](PROCESS.md)。

## 7 个 Module

| # | Module | 时长 | 学完会什么 |
|:-:|:--|:--|:--|
| 01 | [CLAUDE.md · 项目记忆](modules/01-claude-md/) | 25 min | 知道项目记忆怎么写不臃肿、什么进 global 什么进 project，会用 /init 起手 |
| 02 | [Skills · 渐进披露](modules/02-skills/) | 25 min | 会写 SKILL.md frontmatter，理解三层加载（SKILL.md / reference/ / scripts/）省 token 的逻辑 |
| 03 | [Sub-Agents · 子代理](modules/03-subagents/) | 35 min | 知道什么任务该开 sub-agent、怎么配 tools 和 model、看得懂 bug-fix 三段管线 |
| 04 | [Hooks · 事件自动化](modules/04-hooks/) | 25 min | 会写 PreToolUse 拦危险命令、PostToolUse 自动格式化，明白 hooks 不是 Claude 在执行 |
| 05 | [MCP · 外部工具接入](modules/05-mcp/) | 25 min | 会配 .mcp.json 接 filesystem/fetch/memory，知道什么场景该自己写 server |
| 06 | [双轴设计模式](modules/06-dual-axis-patterns/) | 45 min | 拿到一份 agent 源码能用 5 步法拆解，能在双轴矩阵上定位自家系统、识别盲区 |
| 07 | [团队工程化 · Plugins](modules/07-team-engineering/) | 30 min | 会把 commands + agents + skills + hooks + MCP 打包成一个团队 plugin 分发 |

总时长 ≈ 3 小时 30 分钟。半天工作坊一般挑 4-5 个模块，全日工作坊把 7 个都覆盖一遍。

## 历次交付（instances/）

| 客户 | 时间 | 时长 | 选了哪几个 module |
|:--|:--|:--|:--|
| _（占位，每次交付后回填）_ | _yyyy-mm_ | _Xh_ | _M01 + M03 + M06 + ..._ |

每场交付的 AGENDA / 客户 PDF 话题映射 / 调整过的 demo 脚本，都沉淀在 `instances/<client>-<yyyy-mm>/`。下一家客户开始前，先翻翻 instances/ 找最接近的那场，省一半准备时间。

## 给企业培训用：怎么 fork 一份

```
workshop/
├── README.md                ← 你正在看的这页
├── PROCESS.md               ← fork 步骤
├── modules/
│   ├── 01-claude-md/
│   ├── 02-skills/
│   ├── 03-subagents/
│   ├── 04-hooks/
│   ├── 05-mcp/
│   ├── 06-dual-axis-patterns/
│   └── 07-team-engineering/
└── instances/
    └── <client>-<yyyy-mm>/  ← fork 出来的一场具体培训
        ├── AGENDA.md
        ├── how-to-demo.md
        └── slides/
```

完整流程见 [PROCESS.md](PROCESS.md)：四步走（复制 instance → 改 AGENDA → 调 demo → 出 PPT）。

## 跟公库的关系

| 这里有 | 在公库哪里找原件 |
|:--|:--|
| Module 01 demo 引用 | [`02-Memory/projects/01-web-app/`](../02-Memory/) |
| Module 02 demo 引用 | [`04-Skills/projects/03-financial-skill/`](../04-Skills/) |
| Module 03 demo 引用 | [`03-SubAgents/projects/02-test-runner/`](../03-SubAgents/) |
| Module 04 demo 引用 | [`06-Hooks/projects/01-safety-hooks/`](../06-Hooks/) |
| Module 05 demo 引用 | [`07-MCP/projects/01-basic-config/`](../07-MCP/) |
| Module 07 demo 引用 | [`10-Plugins/projects/03-team-toolkit/`](../10-Plugins/) |
| Module 06 文档 + 图 | 私库 `01-Agent设计模式之美/v4/final/`（不在公库） |

modules/ 里**不复制代码**，只引用公库路径。要演示哪段，`cd` 过去跑。
