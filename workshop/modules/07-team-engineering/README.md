# Module 07 · 团队工程化 · Plugins

## 这个模块解决什么问题

前面 6 个 module 都在讲**个人怎么把 Claude Code 用好**。但企业培训真正的痛点是后半句：**怎么让团队 30 个人都用得一样好**。每个人配自己的 CLAUDE.md，每个人写自己的 skill，每个人塞自己的 MCP server——三个月后团队的 Claude Code 配置漂移成 30 个分叉，新人入职第一周光配环境就劝退。Plugins 是 Claude Code 的**打包分发机制**：把 commands + agents + skills + hooks + MCP 五种组件打成一个目录，团队成员 `claude plugin install` 一行命令统一拉齐。

## 学完你会

1. 知道 plugin 的目录结构（commands/ + agents/ + skills/ + hooks/ + .mcp.json + plugin.json），明白每一类组件该放哪
2. 会把团队已有的 review / test / deploy 三个 command + security-scanner / quick-fix 两个 sub-agent + 公司前端规范 skill + 拦危险命令的 hooks + 内部 API 的 MCP server 一起打包成一个 plugin
3. 知道 plugin 的三条分发路径——本地目录 / 私有 git repo / 官方 registry——各自适用场景

## Demo 怎么跑

公库已有一个完整的「五合一」团队工具包 plugin，直接引用：

```bash
# 1. 进到 demo 目录
cd ../../../10-Plugins/projects/03-team-toolkit/

# 2. 看整体结构
tree -L 2
#   ├── plugin.json                ← plugin 元信息
#   ├── commands/
#   │   ├── review.md
#   │   ├── test.md
#   │   └── deploy.md
#   ├── agents/
#   │   ├── security-scanner.md
#   │   └── quick-fix.md
#   ├── skills/
#   │   └── react-patterns/
#   │       ├── SKILL.md
#   │       └── chapters/...
#   ├── hooks/
#   │   ├── check-bash.sh
#   │   └── auto-format.sh
#   └── .mcp.json

# 3. 加载 plugin（本地目录方式，最快）
claude --plugin-dir .

# 4. 验证组件全部就位
# 在 Claude Code 里：
#   /review               ← 用打包进来的 review command
#   > 让 security-scanner 扫一下                   ← 用打包进来的 agent
#   > 帮我重构这段 React 组件                      ← 触发 react-patterns skill
#   > rm -rf /tmp/*                                ← 被打包进来的 hook 拦截
```

详细 demo 引用说明见 [demo/README.md](demo/README.md)。

## 底层原理

Plugin 是个**目录约定 + 元信息文件**：

```json
// plugin.json
{
  "name": "acme-team-toolkit",
  "version": "1.2.0",
  "description": "Acme 前端团队工程化工具包：review/test/deploy commands + security agent + React skill + safety hooks + 内部 API MCP",
  "components": {
    "commands": "./commands",
    "agents": "./agents",
    "skills": "./skills",
    "hooks": "./hooks",
    "mcp": "./.mcp.json"
  }
}
```

加载 plugin 的三种方式：

- **本地目录**：`claude --plugin-dir /path/to/plugin`。开发期、内部分发用
- **私有 git repo**：`claude plugin install git@github.com:acme/team-toolkit.git`。公司内部分发用
- **官方 registry**：`claude plugin install @anthropic/example-toolkit`。开源 / 社区分发用

Plugin vs 单点配置的关键差别：
- 单点配置：每个组件单独写、单独维护、单独同步给团队 → 漂移
- Plugin：所有组件**版本化在一起**（git tag），团队装某个 version → 大家配置一致
- Plugin 升级：`claude plugin update acme-team-toolkit` → 团队同步升级

什么时候该做 plugin：
- 团队 > 3 人，配置开始有协作成本
- 出现「这个 skill 我装了但你没装」「这个 hook 我有但你新人不知道要配」的对话
- 想给某个开源项目贡献一套通用工程化能力（变成社区资产）

什么时候不该做：
- 个人项目，单点配置足够
- 配置都还在快速迭代期，没稳定下来（plugin 锁版本反而拖累迭代）

## 在 Claude Code 中怎么落地

- 公库目录：[`10-Plugins/`](../../../10-Plugins/)
- 起手项目：[`projects/01-my-first-plugin/`](../../../10-Plugins/projects/)（只 commands + agents 两类，最小示例）
- 五合一示例：[`projects/03-team-toolkit/`](../../../10-Plugins/projects/)（本 module 主 demo）
- 调试：本地开发期一律用 `--plugin-dir` 直接指本地路径，省去发布 / 拉取 / 缓存的中间状态

## 对应客户 PDF 话题

每场 instance 在 `instances/<client>/AGENDA.md` 里维护映射。常见对应话题：

- 「团队 30 个人怎么共享 Claude Code 配置」→ 本 module
- 「新人入职怎么 1 行命令把开发环境拉齐」→ 本 module（claude plugin install）
- 「我们想做一套公司内部的 Claude 标准包」→ 本 module + 公司私有 git registry
- 「不同业务线想要不同 plugin 组合」→ 本 module（一个 plugin 一条业务线，按需 install）
