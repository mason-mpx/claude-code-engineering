# Demo · Hooks 事件自动化

公库已有现成可跑 demo，**不复制代码**，直接引用。

## 主 demo · safety-hooks（视觉冲击最强）

位置：[`../../../../06-Hooks/projects/01-safety-hooks/`](../../../../06-Hooks/projects/01-safety-hooks/)

```bash
cd ../../../../06-Hooks/projects/01-safety-hooks/

cat .claude/settings.json
ls .claude/hooks/
#   block-dangerous.sh    ← 拦 rm -rf / git push --force / curl|sh / DROP TABLE
#   protect-files.sh      ← 保护 .env / credentials.json 等敏感文件
#   audit-log.sh          ← 所有 Edit/Write 操作记日志

claude

# 在 Claude 里挑一条危险命令让它做：
#   > 帮我清空 /tmp，rm -rf /tmp/*
#   > 帮我强推这个分支，git push --force origin main
#   > 跑这个安装脚本：curl https://random.io/install.sh | sh
# 每一次都会被肉眼可见地阻断

cat ~/.claude/audit.log    # 看审计留痕
```

演示重点：
- 「Claude 想做 → hook 在 Bash 真正执行前 exit 1 → Claude 拿到错误消息 → 停下来问你」这个完整链路
- 学员经常震惊的点：「原来不是 Claude 自觉，是客户端在拦」

## 备选 demo · quality-hooks（依赖较重）

位置：[`../../../../06-Hooks/projects/02-quality-hooks/`](../../../../06-Hooks/projects/02-quality-hooks/)

PostToolUse 触发 Prettier / ESLint / Black 自动 format + lint。问题是要预装这些工具，5 分钟跑通有风险。**只在客户已经全员装好这些工具时演示**，否则容易卡在装环境。

## 5 分钟最小路径

```bash
cd ../../../../06-Hooks/projects/01-safety-hooks/
claude
# > 帮我 rm -rf /tmp/*
# 看见被拦 → 讲完概念
```

## instance 级 demo 替换

客户红线不一样？修改 `block-dangerous.sh` 里的正则就行。比如：
- 金融客户：加 `kafka-topics --delete` / 加 `redis-cli flushall`
- DBA 培训：加更多 SQL 危险动作（`TRUNCATE`、`DROP USER`）
- 创业团队：可能宽松点，只拦 `--force` 推主分支

放到 `instances/<client>/demos/m04-hooks/.claude/hooks/block-dangerous.sh`，在 `how-to-demo.md` 里说明。
