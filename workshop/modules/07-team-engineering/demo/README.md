# Demo · 团队工程化 · Plugins

公库已有现成可跑 demo，**不复制代码**，直接引用。

## 主 demo · team-toolkit 五合一

位置：[`../../../../10-Plugins/projects/03-team-toolkit/`](../../../../10-Plugins/projects/03-team-toolkit/)

这是公库里**唯一一个把 commands + agents + skills + hooks + MCP 五种组件完整组合在一起**的 demo，正好对应「团队工程化」的故事。

```bash
cd ../../../../10-Plugins/projects/03-team-toolkit/

tree -L 2
# 看 plugin.json 怎么声明组件
cat plugin.json

# 加载（开发期最快方式）
claude --plugin-dir .

# 验证五种组件全部就位
# 在 Claude Code 里逐个试：
#   /review                      ← command 可用
#   > 让 security-scanner 扫一下  ← agent 可用
#   > 重构这段 React 组件         ← skill 触发
#   > rm -rf /tmp/*               ← hook 拦截
#   > （触发 MCP）查内部 API       ← .mcp.json 已挂载
```

演示重点：
- 「前 6 个 module 你看到的每种扩展，这里全在一个目录里」
- 「团队成员 install 这一个 plugin 就一次拉齐」
- 「version 字段 + git tag 让升级可控」

## 备选 demo · 最小 plugin

位置：[`../../../../10-Plugins/projects/01-my-first-plugin/`](../../../../10-Plugins/projects/01-my-first-plugin/)

只有 commands + agents 两类组件，适合作为「plugin 入门」过渡 demo。但教学价值不如 03-team-toolkit 高——后者把团队工程化的完整故事讲完。

## 5 分钟最小路径

```bash
cd ../../../../10-Plugins/projects/03-team-toolkit/
cat plugin.json
claude --plugin-dir .
# /review
```

## instance 级 demo 替换

客户已经有自己的团队工具组合？建议在 `instances/<client>/demos/m07-team-engineering/` 里搭一个客户专属 plugin：
- commands/ 放他们日常想用的快捷命令（/standup、/release-notes、/jira-summary 这种）
- agents/ 放他们的 review / test / scan agent
- skills/ 放他们公司的内部规范包
- hooks/ 放他们的红线拦截
- .mcp.json 接他们的内部 API

这一刻是整场培训的最高潮：**学员看到一个跟自家公司一模一样的 plugin**，立刻就能想象「下周我们也这样打一个」。
