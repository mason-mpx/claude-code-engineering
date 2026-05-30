# Demo · Sub-Agents 子代理

公库已有现成可跑 demo，**不复制代码**，直接引用。

## 主 demo · test-runner（5 分钟跑通）

位置：[`../../../../03-SubAgents/projects/02-test-runner/`](../../../../03-SubAgents/projects/02-test-runner/)

```bash
cd ../../../../03-SubAgents/projects/02-test-runner/

# 装依赖（很轻）
npm install

# 看 sub-agent 配置
cat .claude/agents/test-runner.md

# 启动
claude

# 在 Claude 里：
#   > 让 test-runner 跑一下测试

# 观察主会话 context 里看不到 "PASS src/calculator.test.js  (15.3 s)" 这种几百行 stdout
# 只看到 sub-agent 返回的精炼摘要
```

演示重点：
- 主 context vs sub-agent context 的隔离（拉一个学员上来看主会话 token 占用对比）
- haiku 模型跑这种「读 + 摘要」任务又快又便宜

## 进阶 demo · bug-fix 三段管线

位置：[`../../../../03-SubAgents/projects/05-bugfix-pipeline/`](../../../../03-SubAgents/projects/05-bugfix-pipeline/)

```bash
cd ../../../../03-SubAgents/projects/05-bugfix-pipeline/
ls .claude/agents/
#   detect-agent.md
#   fix-agent.md
#   verify-agent.md

cat tests/services.test.js   # 三处故意埋的 bug
node tests/services.test.js  # 看 3 个 fail

claude
# 在 Claude 里：
#   > 用三段管线把这些 bug 修了
```

演示重点：
- 「一把梭 vs 三段管线」对比——一个 agent 想同时 detect + fix + verify，context 一塞就乱
- 每段 agent 有自己的 model 选择（detect/verify 用 haiku，fix 用 opus）

## 进阶 demo · agent 团队 bug hunt

位置：[`../../../../03-SubAgents/projects/06-agent-teams-bug-hunt/`](../../../../03-SubAgents/projects/06-agent-teams-bug-hunt/)

多个 agent 并行扫一个 buggy-app，最后汇总成一份调查报告。适合时长充裕（35min+）的工作坊压轴 demo。

## 5 分钟最小路径

```bash
cd ../../../../03-SubAgents/projects/02-test-runner/
npm install && claude
# > 让 test-runner 跑测试
```

## instance 级 demo 替换

客户的栈不是 Node.js？把 `instances/<client>/demos/m03-subagents/` 放一份对应栈的小项目（python pytest / java junit / go test），sub-agent 配置文件结构不变，只换 `tools` 里 Bash 命令。
