# Demo · CLAUDE.md 项目记忆

公库已有现成可跑 demo，**不复制代码**，直接引用。

## 主 demo · React 电商精简正例

位置：[`../../../../02-Memory/projects/01-web-app/`](../../../../02-Memory/projects/01-web-app/)

```bash
cd ../../../../02-Memory/projects/01-web-app/
cat CLAUDE.md       # 看一份「该有的样子」的 CLAUDE.md
claude              # 启动会话
# 在 Claude 里问：「现在这个项目用什么技术栈？组件应该放哪？」
# 验证它给出 React 18 + TS + Vite + 你定义的目录约定
```

演示重点：
- 「冗余信息没有 / 关键约定全有 / 一眼能读完」
- 让学员对比自己手上项目的「靠口口相传的规范」

## 对照 demo · 630 行臃肿反例 + 重构版

位置：[`../../../../02-Memory/projects/03-bloated-claude-md/`](../../../../02-Memory/projects/03-bloated-claude-md/)

```bash
cd ../../../../02-Memory/projects/03-bloated-claude-md/
wc -l CLAUDE.md                # 630 行
wc -l CLAUDE.refactored.md     # 精简后 ~80 行
diff CLAUDE.md CLAUDE.refactored.md | head -40
```

演示重点：
- 「先讲哪 5 类内容是冗余（项目背景故事 / 历史决策 / 完整 API 文档 / 团队成员介绍 / 长篇风格指南）」
- 再讲为什么 Claude 读到这种文件反而表现变差（信号被噪声淹没）

## 5 分钟最小路径

```bash
# 路径 A · 只跑正例（90 秒）
cd ../../../../02-Memory/projects/01-web-app/ && claude

# 路径 B · 正例 + 反例对照（3-4 分钟）
cd ../../../../02-Memory/projects/01-web-app/ && cat CLAUDE.md
cd ../../03-bloated-claude-md/ && wc -l CLAUDE.md CLAUDE.refactored.md
```

## instance 级 demo 替换

如果客户给了自家代码片段（脱敏后），在 `instances/<client>/demos/m01-claude-md/` 放一份替换版的 CLAUDE.md，然后在 `how-to-demo.md` 里把上面的 `cd` 路径改过去。通用 module 不动。
