# 怎么 fork 给下一家企业

每一场企业培训都是**这套积木的一次具体组合**。客户不一样，话题不一样，时长不一样，选的 module 不一样，demo 用的代码也得跟客户的技术栈贴一点。

这份文档讲的就是这个 fork 流程：四步走完，半天的准备工作能压到两小时。

## 步骤 1 · 复制最像的那一场 instance

先翻 `instances/` 找一场跟新客户最接近的：
- 行业接近（金融 / 互联网 / 测试设备 / 教育）
- 时长接近（半天 / 全天 / 两天）
- 学员画像接近（资深工程师 / 初中级 / 业务岗）

```bash
cd workshop/
ls instances/

# 假设新客户是 acme-corp 2026 年 7 月一场半天工作坊
# 最像的是 keysight-2026-05/（同是设备/测试行业、半天）
cp -r instances/keysight-2026-05/ instances/acme-corp-2026-07/
cd instances/acme-corp-2026-07/
```

如果还没有任何 instance，从 `modules/` 直接复制一份模板（每个 module 的 README 自带 AGENDA 可参考的片段）。

## 步骤 2 · 改 AGENDA.md（客户话题 → modules 映射）

每个客户会给一份**他们自己想听什么**的 PDF 或邮件。打开 `AGENDA.md`，把客户的话题列出来，一条一条映射到 `modules/` 里的某个 module：

```markdown
| 客户话题（PDF 第几页） | 对应 module | 时长 | 备注 |
|:--|:--|:--|:--|
| "怎么让 Claude 记住我们的代码规范"（p3） | M01 CLAUDE.md | 25min | 用客户的 React 栈做 demo |
| "能不能让 Claude 自动跑测试"（p4） | M03 Sub-Agents | 35min | test-runner 换成客户的 jest 配置 |
| "怎么不让它把生产数据库 drop 掉"（p5） | M04 Hooks | 25min | 加一条客户 schema 的拦截 |
| "团队怎么共用这套配置"（p6） | M07 Plugins | 30min | — |
| _（客户没问到但应该讲）_ 双轴看自家系统 | M06 | 30min | 截断版，只讲矩阵 + 5 步法 |
```

**话题对不上 module 的两种处理**：
- 客户问的是某个 module 里的细节 → 把对应 module 的时长拉长 5-10 分钟，在那段加细节
- 客户问的话题哪个 module 都不沾边 → 在 instance 目录下新建 `custom-segments/<topic>.md`，单独写一段，不要污染通用 module

## 步骤 3 · 调 demo（how-to-demo.md）

`how-to-demo.md` 是讲师当天**对着电脑跑 demo 的小抄**。每场都得改，因为：
- 客户的栈不一样（React/Vue/Spring/Django…）
- 客户的代码库结构不一样
- 客户敏感数据不能展示，要换 dummy

每个 module 的 demo 引用了公库 `02-Memory/`、`04-Skills/` 等目录。改动方式有两种：

**轻改 · 同一份 demo 换数据**
```bash
# 比如 M01 demo 用的是 React 电商，客户是 Vue 仪表盘
cp -r ../../../02-Memory/projects/01-web-app/ ./demos/m01-claude-md/
# 把 CLAUDE.md 里 React/TS/Vite 三段换成 Vue/JS/Webpack
vim ./demos/m01-claude-md/CLAUDE.md
```

**重改 · 客户提供了自己的代码片段**
- 客户发了一份脱敏的代码（很常见）
- 放到 `instances/<client>-<yyyy-mm>/demos/<module-id>/`
- 在 `how-to-demo.md` 里写清楚：M01 demo 不走公库默认，走 ./demos/m01-claude-md/

`how-to-demo.md` 模板已经在 `instances/keysight-2026-05/`（或最近一场 instance）里——直接拷过来改。

## 步骤 4 · 复用 PPT 模板或新建

PPT 不是必需。三种交付模式：

**模式 A · 全 markdown，不出 PPT**
- 适合小团队 workshop、围圈讲、屏幕共享 markdown 也能讲
- 每个 module 的 `slides.md` 直接用，配 speaker notes
- 准备最快，0 PPT 工作量

**模式 B · 复用之前的 PPT 模板**
- 客户要正式 PPT、要 logo、要打印版
- 从最像的那场 instance 拷 `slides/*.pptx`
- 改 logo / 改客户名 / 删掉不讲的 module 对应 slide
- 一般 1-2 小时搞定

**模式 C · 新建 PPT**
- 完全新客户、风格要求差很多（比如对方品牌色完全冲突）
- 走公库 [`04-Packt-ClaudeCodeEngineering/`](../04-Packt-ClaudeCodeEngineering/) 的 Marp 单文件路线
  - `slides.md` 为唯一源，`build.sh` 产 HTML/PDF/PPTX 三件套
  - speaker notes 嵌在 `<!-- Speaker Notes: ... -->` 注释里
- 第一次走这条路 4-6 小时；之后 fork 就快

## 步骤 5（可选）· 交付后回填 instance + 主 README

培训完那天或第二天，做两件事：

1. 把当天用过的 AGENDA / how-to-demo / 客户反馈 / 自己事后想到的修订点，提交到 `instances/<client>-<yyyy-mm>/`
2. 回到 `workshop/README.md`，在「历次交付」表里加一行

这一步看似可有可无，**但下一家客户的准备工作能不能压到两小时，全靠它**。

## 反面清单（不要做的事）

- 不要直接改 `modules/` 里的通用 README 来适配某一家客户 → 改去 `instances/<client>/`
- 不要把客户敏感数据、内部代码 commit 进公库 → instances/ 里准备一个 .gitignore，必要时整个 instance 目录都不 push
- 不要为了一家客户把 module 拆掉重组 → 拆出来的部分放 `instances/<client>/custom-segments/`，通用骨架保持稳定
- 不要把 PPT 当唯一交付物 → markdown + demo 才是真主线，PPT 只是辅助
