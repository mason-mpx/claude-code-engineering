# Module 05 · MCP 外部工具接入

## 这个模块解决什么问题

Claude 自带的工具就那几个（Read / Write / Edit / Bash / Glob / Grep）。但真实工作里你需要它能查数据库、调内部 API、读 Jira ticket、看 Sentry 错误、操作 K8s。把这些能力都塞进 Claude 本体既不现实也不安全。MCP（Model Context Protocol）就是 Anthropic 设计的**标准外部工具接入协议**：你把工具部署成一个 MCP server，Claude Code 启动时通过 `.mcp.json` 连接，server 暴露的工具就变成 Claude 可调用的工具。

## 学完你会

1. 看得懂 `.mcp.json` 的配置语法，知道 stdio / http / sse 三种传输方式各自什么场景
2. 会用三个零依赖 MCP server（filesystem / fetch / memory）做 demo，明白 MCP 不是「难配的高级东西」
3. 知道什么时候该用现成 MCP server、什么时候该自己写一个（boundary 判断）

## Demo 怎么跑

公库已有零依赖配置，直接引用：

```bash
# 1. 进到 demo 目录
cd ../../../07-MCP/projects/01-basic-config/

# 2. 看 .mcp.json
cat .mcp.json
#   filesystem  → 读写指定目录
#   fetch       → HTTP 抓页面
#   memory      → 临时 key-value 存储

# 3. 验证 MCP 连接
claude mcp list
# 应该看到三个 server 状态为 connected

# 4. 启动 Claude Code，让它用 MCP 工具
claude
# 在 Claude 里说：
#   > fetch https://example.com 然后把 title 存到 memory
# 观察 Claude 调用的是 mcp__fetch__fetch 和 mcp__memory__set 这种带 mcp__ 前缀的工具
```

详细 demo 引用说明见 [demo/README.md](demo/README.md)。

## 底层原理

`.mcp.json` 配置结构：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/dir"]
    },
    "fetch": {
      "command": "uvx",
      "args": ["mcp-server-fetch"]
    }
  }
}
```

启动流程：
1. Claude Code 启动 → 读 `.mcp.json` → 按配置启动每个 MCP server 进程
2. 每个 server 通过 stdio（或 HTTP / SSE）跟 Claude Code 通信
3. Server 报告自己暴露了哪些 tool（带 schema）
4. Claude Code 把这些 tool 包装成 `mcp__<server>__<tool>` 的命名空间塞进可用工具列表
5. Claude 调用 → Claude Code 转发到对应 server → server 执行 → 返回结果

三种传输方式：
- **stdio**（最常见）：server 是个本地进程，通过标准输入输出通信。适合本地能力（文件系统、本地数据库）
- **http**：server 是远程 HTTP 服务。适合内部 API、SaaS 接入
- **sse**：server 推送流式响应。适合长任务、订阅类场景

MCP server 在哪里找：
- 官方注册表：[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
- 一类工业 server（filesystem / fetch / memory / postgres / sqlite / github / slack）官方维护
- 二类社区 server（jira / linear / sentry / k8s / 各种 SaaS）质量参差，要 review
- 自己写：[`projects/02-custom-server/`](../../../07-MCP/projects/) 是 TS 写 server 的教学样本

自写 vs 用现成的判断：
- 现成的覆盖了你的能力 → 直接用
- 现成的接近但缺一两个工具 → fork 现成的改
- 完全是公司内部能力（内部 API、自研系统）→ 自己写

## 在 Claude Code 中怎么落地

- 公库目录：[`07-MCP/`](../../../07-MCP/)
- 配置位置：
  - 项目级 `.mcp.json`（团队共享）
  - 用户级 `~/.config/claude/mcp.json`（个人）
- 调试命令：`claude mcp list` 看连接状态，`claude mcp logs <server>` 看 server 日志
- 进阶 demo：[`projects/02-custom-server/`](../../../07-MCP/projects/) 用 TypeScript 写一个自己的 MCP server

## 对应客户 PDF 话题

每场 instance 在 `instances/<client>/AGENDA.md` 里维护映射。常见对应话题：

- 「能不能让 Claude 查我们公司的 Jira / Confluence / 内部系统」→ 本 module
- 「想让它接公司的数据库」→ 本 module（postgres / sqlite MCP server）
- 「我们想自己写一个 MCP server 让全公司用」→ 本 module 进阶 + 02-custom-server demo
