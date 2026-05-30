# Demo · MCP 外部工具接入

公库已有现成可跑 demo，**不复制代码**，直接引用。

## 主 demo · 零依赖三 server

位置：[`../../../../07-MCP/projects/01-basic-config/`](../../../../07-MCP/projects/01-basic-config/)

```bash
cd ../../../../07-MCP/projects/01-basic-config/
cat .mcp.json
#   filesystem  ← 读写指定本地目录
#   fetch       ← HTTP 抓页面
#   memory      ← 临时 KV 存储
# 三个都不要 API key，5 分钟跑通

claude mcp list             # 看三个 server 状态
claude
# > fetch https://example.com，把 title 存到 memory
# > 再从 memory 里读出来给我
```

演示重点：
- 「MCP 不神秘，本质就是个进程间通信协议 + 工具白名单」
- 看 Claude 调用的工具名是 `mcp__fetch__fetch` 这种带前缀的——清楚知道哪些是 Claude 原生工具、哪些是外部 MCP 给的

## 进阶 demo · 自写 MCP server

位置：[`../../../../07-MCP/projects/02-custom-server/`](../../../../07-MCP/projects/02-custom-server/)

```bash
cd ../../../../07-MCP/projects/02-custom-server/
npm install
npm run build
# 看 src/index.ts 怎么用 @modelcontextprotocol/sdk 注册一个工具
```

演示重点：
- 「写一个 MCP server 比想象简单 50 行 TS」
- 适合时长充裕、客户技术栈是 Node.js / TS 的场景；如果是 Python 客户改演示 [modelcontextprotocol/python-sdk]

## 5 分钟最小路径

```bash
cd ../../../../07-MCP/projects/01-basic-config/
claude mcp list && claude
# > fetch https://example.com
```

## instance 级 demo 替换

客户已经有 Jira / Confluence / 自家 API？常见做法：
- 在 `instances/<client>/demos/m05-mcp/.mcp.json` 加客户的 server
- 如果客户那个内部 server 还没写 → 把 02-custom-server 复制改一份当 demo
- API key / token 走环境变量，**不要硬编码**进 .mcp.json 提交到客户仓库
