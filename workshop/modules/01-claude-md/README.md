# Module 01 · CLAUDE.md 项目记忆

## 这个模块解决什么问题

每次新开会话，Claude 都不知道你的项目用什么栈、走什么规范、有哪些约定。要么你每次都讲一遍（费 token、费时间、还容易讲漏），要么它瞎猜（猜错的代价比讲一遍大得多）。CLAUDE.md 就是那份**项目记忆契约**——一次写好，每次会话自动加载，让 Claude 在第一秒就处于「已经熟悉这个项目」的状态。

## 学完你会

1. 知道 global / project / 子目录三层 CLAUDE.md 各自该放什么内容，不会把 global 写成项目说明书、也不会把 project 写成个人偏好集
2. 会用 `/init` 命令从零起手生成一份骨架，然后基于真实项目结构补血
3. 能识别一份 CLAUDE.md 是「精简够用」还是「臃肿失控」，知道 630 行的反例错在哪里

## Demo 怎么跑

公库已有可跑 demo，直接引用：

```bash
# 1. 进到 demo 目录
cd ../../../02-Memory/projects/01-web-app/

# 2. 看一眼 CLAUDE.md 长什么样
cat CLAUDE.md

# 3. 启动 Claude Code
claude

# 4. 验证它读到了项目记忆
# 在 Claude Code 里问：
#   > 现在这个项目的技术栈是什么？组件应该放哪个目录？

# 5. （可选）对照看反例
cd ../03-bloated-claude-md/
wc -l CLAUDE.md   # 630 行 — 一眼能看出哪些是冗余
# 同目录还有 CLAUDE.refactored.md，是精简后的版本，对照读
```

详细 demo 引用说明见 [demo/README.md](demo/README.md)。

## 底层原理

CLAUDE.md 不是黑魔法。它就是一份会话启动时被自动注入到 system context 里的 markdown。Claude Code 在启动会话时按顺序读取三层文件：

1. **Global**（`~/.claude/CLAUDE.md`）—— 跨所有项目的个人习惯：「我喜欢简洁回答」「commit message 用中文」这种
2. **Project**（项目根目录 `CLAUDE.md`）—— 这个项目独有的：技术栈、目录约定、命名规范、跑测试的命令
3. **Subdirectory**（深层目录里的 `CLAUDE.md`）—— 某个子模块独有的细节（不常用，但深仓库里有用）

三层合并后塞进 system prompt。这意味着两件事：
- **每个 token 都要付费**——CLAUDE.md 越长，每次会话的成本越高、context window 越紧
- **写得越具体，效果越好**——「保持代码整洁」是废话，「函数超过 50 行先停下来问我要不要拆」是真规则

「3 个问题框架」判断一段内容该不该进 CLAUDE.md：
1. 这条规则**每个新会话**都需要 Claude 知道吗？（如果只是一次性任务，写在 prompt 里就行）
2. 这条规则**Claude 自己看代码看不出来**吗？（如果代码里写着 `import { useState }`，CLAUDE.md 不用再说「用 React Hooks」）
3. 这条规则**违反了会引起明显问题**吗？（如果违反了也无所谓，那不是规则，是偏好，放 global）

三个都是 yes，才进 project CLAUDE.md。

## 在 Claude Code 中怎么落地

- 公库目录：[`02-Memory/`](../../../02-Memory/)
- 起手命令：在项目根目录运行 `/init`，Claude 会扫一遍项目然后生成 CLAUDE.md 草稿
- 维护节奏：每次发现自己又给 Claude 讲了一遍同一件事 → 那件事就该进 CLAUDE.md
- 复盘命令：`/memory` 查看当前会话加载了哪些记忆文件

## 对应客户 PDF 话题

写明本次 instance 的 PDF 第几页问到了 CLAUDE.md。例如：

- 是德 2026-05 培训 PDF p3「怎么让 Claude 记住我们的代码规范」→ 本 module
- 是德 2026-05 培训 PDF p7「新人入职怎么让 Claude 快速进入项目状态」→ 本 module（补一段 onboarding 用法）

每场 instance 在 `instances/<client>/AGENDA.md` 里维护这张映射。
