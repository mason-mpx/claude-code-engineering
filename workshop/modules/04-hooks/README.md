# Module 04 · Hooks 事件自动化

## 这个模块解决什么问题

Claude 是个聪明但不知道你公司红线在哪的实习生。它可能在没多想的情况下执行 `rm -rf /tmp/foo`，可能 `git push --force` 到 main，可能在你不希望的时候碰生产数据库。靠提醒它「记住不要..."不可靠——人类自己都会忘。Hooks 是 Claude Code 给你的**事件钩子**：在工具调用前（PreToolUse）、调用后（PostToolUse）、会话开始/结束等关键节点，自动跑你写的 shell 脚本。**不是 Claude 在执行，是 Claude Code 这个客户端在执行**——这是 hooks 跟 sub-agent / skill 最大的区别。

## 学完你会

1. 知道 hooks 跟 skills / sub-agents 的本质区别：hooks 是**客户端在拦截**，不依赖 Claude 自己「记得」遵守
2. 会写 PreToolUse hook 拦危险命令（rm -rf、git push --force、curl | sh、DROP TABLE），会写 PostToolUse hook 做后处理（自动 format、auto lint）
3. 看得懂 `.claude/settings.json` 的 hook 配置语法，知道 matcher / command / blocking 这几个字段各自起什么作用

## Demo 怎么跑

公库已有可跑 demo，直接引用：

```bash
# 1. 进到 demo 目录
cd ../../../06-Hooks/projects/01-safety-hooks/

# 2. 看 hook 配置
cat .claude/settings.json

# 3. 看三个拦截脚本各自干什么
cat .claude/hooks/block-dangerous.sh
cat .claude/hooks/protect-files.sh
cat .claude/hooks/audit-log.sh

# 4. 启动 Claude Code
claude

# 5. 让 Claude 试着干件危险事
# 在 Claude 里说：
#   > 帮我清理一下 /tmp 目录，用 rm -rf /tmp/* 应该可以
# 观察 Claude 调用 Bash 之前被 hook 阻断、拿到错误消息后停下来问你

# 6. 看审计日志
cat ~/.claude/audit.log
```

详细 demo 引用说明见 [demo/README.md](demo/README.md)。

## 底层原理

Hooks 配置在 `.claude/settings.json` 里：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": ".claude/hooks/block-dangerous.sh",
        "blocking": true
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "command": ".claude/hooks/audit-log.sh",
        "blocking": false
      }
    ]
  }
}
```

四个关键字段：
- **事件名**（PreToolUse / PostToolUse / SessionStart / Stop 等）：什么时刻触发
- **matcher**：只对哪些工具触发（正则匹配 tool name）
- **command**：跑什么脚本。脚本通过 stdin 拿到 tool input JSON，通过 exit code 表态（0 = 放行，非 0 = 拦截）
- **blocking**：拦截能不能挡住工具实际执行（true）还是只记日志（false）

为什么 hooks 比「让 Claude 遵守规则」可靠：
- Claude 是 LLM，让它「永远不执行 rm -rf」是概率事件——大多数时候它确实不会，但**少数情况下它会**
- Hooks 是 deterministic 的——脚本里写了 `if grep -q "rm -rf /" <<<"$input"; then exit 1; fi`，**100% 会拦**
- 这就是为什么生产环境的 Claude Code 一定要配 hooks，不能只靠 Claude 自觉

PreToolUse vs PostToolUse 的设计意图：
- PreToolUse：**拦截**（危险命令在执行前停下，blocking=true）
- PostToolUse：**响应**（执行完后自动 format、记日志、发通知，通常 blocking=false）

## 在 Claude Code 中怎么落地

- 公库目录：[`06-Hooks/`](../../../06-Hooks/)
- 配置位置：
  - 项目级 `.claude/settings.json`（团队共享，进 git）
  - 用户级 `~/.claude/settings.json`（个人，不进 git）
- 调试命令：在 hook 脚本里 `echo` 到 stderr，Claude Code 启动时加 `--debug` 能看到 hook 触发日志
- 进阶 demo：[`projects/02-quality-hooks/`](../../../06-Hooks/projects/) auto-format + lint + run-tests（依赖 Prettier/ESLint/Black，跑通门槛比 safety-hooks 高）

## 对应客户 PDF 话题

每场 instance 在 `instances/<client>/AGENDA.md` 里维护映射。常见对应话题：

- 「怎么防止 Claude 误删 / 误推生产 / 碰生产数据库」→ 本 module
- 「能不能让它自动跑 lint / format」→ 本 module（PostToolUse）
- 「我们公司有审计合规要求，所有 Claude 操作要留痕」→ 本 module（audit-log hook）
