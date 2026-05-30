# Demo · Skills 渐进披露

公库已有现成可跑 demo，**不复制代码**，直接引用。

## 主 demo · financial-analyzing 三层结构

位置：[`../../../../04-Skills/projects/03-financial-skill/`](../../../../04-Skills/projects/03-financial-skill/)

```bash
cd ../../../../04-Skills/projects/03-financial-skill/
tree -L 2
#   .
#   ├── SKILL.md                       ← 入口 1.4 KB
#   ├── reference/
#   │   ├── revenue.md                 ← 详细 1.5 KB
#   │   ├── costs.md
#   │   └── profitability.md
#   ├── templates/
#   │   └── analysis_report.md
#   └── scripts/
#       └── calculate_ratios.py

mkdir -p ~/.claude/skills/financial-analyzing/
cp -r * ~/.claude/skills/financial-analyzing/

claude
# 在 Claude 里说：「analyze revenue growth」
# 观察哪些文件被 Read、哪些没被 Read
```

演示重点：
- 「触发前 token 成本 ~150（只是 description 那行）」
- 「触发后 ~1500（SKILL.md），按需读 reference 再 +1500」
- 「把全部内容写进 CLAUDE.md 的话每次都付 ~5000」

## 备选 demo · 最小 skill

位置：[`../../../../04-Skills/projects/00-basic-skill/`](../../../../04-Skills/projects/00-basic-skill/)

只有 SKILL.md，没 reference/ 也没 scripts/。适合教学开头先看一眼「最小可工作 skill 长什么样」，再到 financial-skill 看「三层架构怎么扩」。

## 5 分钟最小路径

```bash
cd ../../../../04-Skills/projects/03-financial-skill/
cat SKILL.md                  # 看 frontmatter + 入口指引（30 秒）
ls reference/ templates/ scripts/   # 看三层结构（30 秒）
# 部署 + 启动 + 触发（3 分钟）
mkdir -p ~/.claude/skills/financial-analyzing/ && cp -r * ~/.claude/skills/financial-analyzing/
claude
```

## instance 级 demo 替换

如果客户的能力包是法务合规、是医疗病历分析、是测试用例生成——把对应 skill 放到 `instances/<client>/demos/m02-skills/<custom-skill>/`，在 `how-to-demo.md` 里改一下 `cd` 路径。三层结构（SKILL.md / reference/ / scripts/）保持不变，那是模板。
