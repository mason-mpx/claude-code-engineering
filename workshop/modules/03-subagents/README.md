# Module 03 · Sub-Agents 子代理

## 这个模块解决什么问题

主会话的 context 是宝贵稀缺资源。当一个任务又长又脏（跑测试输出几千行日志、扫一遍代码库找所有 TODO、把一份文档翻译完），全塞在主 context 里有两个代价：① 主会话的 token 被填满，主线思考没空间；② 输出冗长，关键结论被噪声埋掉。Sub-Agent 是一个**带独立上下文 + 独立工具集 + 独立 system prompt** 的子进程，做完任务只返回一份精炼摘要给主会话。

## 学完你会

1. 知道什么任务该开 sub-agent（高噪声、可独立完成、不需要持续对话），什么任务别开（轻量任务直接做反而更快）
2. 会写 sub-agent 配置文件：`name / description / tools / model` 四个关键字段各自怎么定
3. 看得懂 bug-fix 三段管线（detect → fix → verify 三个 sub-agent 串成一条链）为什么比一个 agent 一把梭更可靠

## Demo 怎么跑

公库已有可跑 demo，直接引用：

```bash
# 1. 进到 demo 目录
cd ../../../03-SubAgents/projects/02-test-runner/

# 2. 装依赖（极少）
npm install

# 3. 看 sub-agent 配置
cat .claude/agents/test-runner.md
#   ---
#   name: test-runner
#   description: Use when the user wants to run the test suite and get a clean pass/fail summary
#   tools: Read, Bash, Glob, Grep
#   model: claude-haiku-4-5
#   ---

# 4. 启动 Claude Code
claude

# 5. 让 sub-agent 跑测试
# 在 Claude 里说：
#   > 让 test-runner 跑一下测试，告诉我结果

# 6. 观察：sub-agent 在隔离 context 里跑了 npm test、看完几百行日志，
#    返回主会话的只是 "23 passed / 2 failed，失败原因 X 和 Y" 这样的摘要
```

详细 demo 引用说明见 [demo/README.md](demo/README.md)。

## 底层原理

Sub-Agent 是 Claude Code 的一等扩展机制。配置文件结构：

```yaml
---
name: test-runner
description: Use when the user wants to run the test suite and get a clean pass/fail summary
tools: Read, Bash, Glob, Grep        # ← 这个 sub-agent 能用的工具白名单（默认全开）
model: claude-haiku-4-5               # ← 用哪个模型跑（便宜模型扛重复脏活）
---

# System prompt for this sub-agent
You are a focused test runner. Run the test suite, parse the output, and return a clean summary:
- 总数 / passed / failed
- 失败的测试名 + 一句话失败原因
不要返回完整日志，主会话不需要看几千行 stdout。
```

四个字段的设计意图：
- **name**：主会话怎么呼叫它（"让 test-runner 去跑..."）
- **description**：路由依据，Claude 看这句判断该不该把任务交给它
- **tools**：权限边界，sub-agent 只能用列出来的工具（关键安全约束）
- **model**：成本控制，脏活用 haiku，重思考用 opus

主会话调用 sub-agent 的流程：
1. 主会话把任务包装成一个 prompt 发给 sub-agent
2. Sub-agent 启动新 context，跑自己的工作循环（可能调多次工具）
3. Sub-agent 完成后只返回最终结论给主会话
4. 主会话基于结论继续思考，sub-agent 跑过程中产生的几千行日志**不进入主 context**

Bug-fix 三段管线为什么可靠：
- detect-agent 用 haiku 扫全库找 bug → 输出 bug 清单（隔离的）
- fix-agent 用 opus 针对 bug 清单逐个修 → 输出 patch（隔离的）
- verify-agent 用 haiku 跑回归测试确认修对了 → 输出 pass/fail（隔离的）
- 三个 agent 各自的失败不会污染另外两个的 context，主会话只看三段摘要

## 在 Claude Code 中怎么落地

- 公库目录：[`03-SubAgents/`](../../../03-SubAgents/)
- 配置位置：`.claude/agents/<agent-name>.md`（项目级）或 `~/.claude/agents/<agent-name>.md`（用户级）
- 调用方式：直接在 prompt 里点名「让 test-runner 跑...」，或 Claude 根据 description 自动路由
- 进阶 demo：[`projects/05-bugfix-pipeline/`](../../../03-SubAgents/projects/) 三段管线 / [`06-agent-teams-bug-hunt/`](../../../03-SubAgents/projects/) 多 agent 团队

## 对应客户 PDF 话题

每场 instance 在 `instances/<client>/AGENDA.md` 里维护映射。常见对应话题：

- 「自动跑测试 / 自动扫代码 / 自动生成发布说明」→ 本 module
- 「不想让 Claude 一次做太多事情失控」→ 本 module（任务拆分到 sub-agent）
- 「便宜模型干粗活、贵模型干细活」→ 本 module（model 字段的成本设计）
