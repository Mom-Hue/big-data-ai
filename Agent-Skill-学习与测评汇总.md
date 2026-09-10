# Agent Skill 学习与测评汇总

> **一份文档看懂 Agent Skill：从概念学习 → 技能包解剖 → 分层测评 → 成果数据**
>
> 课程：大数据与人工智能 ｜ 作者：Mom-Hue ｜ 更新：2026-09-10
> 所有知识点的权威来源见文末「附录 A」，正文关键处亦直接标注来源。

---

## 目录

- [第一部分 · 学习篇：Agent Skill 是什么](#第一部分--学习篇agent-skill-是什么)
- [第二部分 · 实战篇：一个技能包的解剖](#第二部分--实战篇一个技能包的解剖)
- [第三部分 · 测评篇：分层自适应测验（含答案与解析）](#第三部分--测评篇分层自适应测验含答案与解析)
- [第四部分 · 成果篇：学习数据看板](#第四部分--成果篇学习数据看板)
- [附录 A · 权威来源清单](#附录-a--权威来源清单)
- [附录 B · 术语表](#附录-b--术语表)

---

## 第一部分 · 学习篇：Agent Skill 是什么

### 1.1 一句话定义

**Agent Skill（智能体技能）** 是一个"文件夹 + 说明书"形式的技能包：把某类任务的**专业流程、领域知识、脚本工具**打包成一个目录，让 AI 智能体在需要时自动发现并加载，从而把"通用助手"变成"领域专家"。

> 📖 来源：Anthropic 官方文档《Agent Skills Overview》—— https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview

### 1.2 为什么需要它

- AI 大模型虽然聪明，但**缺少"真实工作环境的过程性知识"**（procedural knowledge）与组织、业务上下文；
- 以前这些知识要么存在人的脑子里反复口头交代，要么写成超长提示词每次全量加载；
- Skill 把知识**打包成可版本管理、可移植、按需加载**的文件，随取随用。

> 📖 来源：Claude 官方博客《Equipping agents for the real world with Agent Skills》—— https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills

### 1.3 技能包的标准结构

一个技能 = 一个目录，**至少包含一个 SKILL.md**：

```
skill-name/
├── SKILL.md              # 必需：YAML 元信息 + 操作指令
├── scripts/              # 可选：可执行代码（Python/Bash…）
├── references/           # 可选：按需加载的参考资料/文档
├── assets/               # 可选：输出用模板/图片/字体等
└── ...（可再扩展其他文件）
```

目录名 = 技能名 = SKILL.md 中 `name` 字段，三者必须一致。

> 📖 来源：agentskills.io 开放标准 —— https://agentskills.io/ ｜ 规范 —— https://agentskills.io/specification

### 1.4 SKILL.md 与 frontmatter 规范

SKILL.md 顶部用 `---` 包裹的 YAML **frontmatter** 元信息是"门牌号"：

```yaml
---
name: your-skill-name        # 必填：小写字母/数字/连字符，≤64字符，不能含 "anthropic"/"claude"
description: 说明做什么+何时用  # 必填：≤1024字符，agent 靠它判断何时触发本技能
---
# 技能标题
## Instructions   指令正文（清晰的分步指引）
## Examples       用法示例
```

| 字段 | 必填 | 约束 |
|------|------|------|
| `name` | 是 | 1–64 字符，仅小写字母、数字、连字符；不能有连续连字符；须与目录名一致 |
| `description` | 是 | 1–1024 字符，须同时说明"做什么"和"何时用" |
| `license` / `compatibility` / `metadata` | 否 | 可选扩展字段 |

> **description 是技能最重要的字段** —— 它决定了 agent 会不会在正确时机激活这个技能。

> 📖 来源：Anthropic 官方文档（同 1.1）｜ agentskills.io 规范

### 1.5 核心机制：渐进式披露（Progressive Disclosure）

这是 Agent Skill 高效的关键，分三层按需加载：

| 层级 | 内容 | 何时进入上下文 |
|------|------|----------------|
| 第 1 层 | 技能 `name` + `description` | 会话启动即加载（约 100 tokens/技能） |
| 第 2 层 | SKILL.md 正文指令 | 任务与 description 匹配时才读取 |
| 第 3 层 | references / scripts / assets | 执行中确有必要才加载或运行 |

好处：可以同时装几十个技能，上下文只承担"被用到的那几个"的开销。

> 📖 来源：Claude 官方博客（同 1.2）｜ Anthropic 官方文档（同 1.1）

### 1.6 工作流：技能如何被使用

1. **发现（Discovery）**：启动时 agent 只读所有技能的 name / description；
2. **激活（Activation）**：当前任务与某个技能的 description 匹配 → agent 读取该技能完整 SKILL.md；
3. **执行（Execution）**：按指令操作，必要时运行 scripts 或读取 references。

> 📖 来源：agentskills.io 开放标准

### 1.7 何时应该创建一个 Skill

官方建议，出现以下信号就值得把内容沉淀为技能：

- 反复把**同一套脚本、清单、多步流程**粘贴进对话；
- 项目说明文件（如 CLAUDE.md）某部分已经从"事实"长成了"操作流程"；
- 希望把某领域的**专家做法固化**并分享给团队复用。

> 📖 来源：Claude Code 文档《使用 skills 扩展 Claude》—— https://code.claude.com/docs/zh-TW/skills

### 1.8 生态与开放标准

- **2025-10**：Anthropic 在 Claude Code / Claude 应用中推出 Agent Skills；
- **2025-12-18**：发布为**开放标准**，官网 agentskills.io，参考实现与示例开源在 GitHub；
- 已被 Claude Code、OpenAI Codex、GitHub Copilot、Cursor、VS Code 等众多工具采纳，技能可跨产品复用。

> **与 MCP 的关系**：MCP 管"智能体如何连上工具和数据"（给手），Skill 管"智能体该怎么做一件事"（给方法）。两者互补。

> 📖 来源：agentskills.io ｜ Anthropic 开源示例技能库 —— https://github.com/anthropics/skills

### 1.9 安全须知（务必了解）

技能本质是"指令 + 可能被执行的代码"，因此：

- **只用可信来源**的技能（自己写的、官方或知名组织发布的）；
- 使用前**审计捆绑文件**（SKILL.md、脚本、图片）—— 警惕意外网络请求、异常文件访问、与描述不符的操作；
- **从外部 URL 拉取内容的技能风险更高**，其内容可能被篡改注入恶意指令；
- 涉及敏感数据 / 生产系统的技能要格外谨慎。

> 📖 来源：Anthropic 官方文档"安全注意事项"章节

### 1.10 学习篇知识地图

```
Agent Skill
├─ 是什么 → 目录 + SKILL.md（指令/脚本/资源的组合包）
├─ 为什么 → 给 agent 过程性知识与领域上下文
├─ 结构   → SKILL.md(必需) + scripts/ references/ assets/(可选)
├─ 门牌号 → frontmatter 的 name + description
├─ 机制   → 渐进式披露：name/desc → 全文 → 按需资源
├─ 何时建 → 步骤重复粘贴 / 说明文件膨胀为流程
├─ 生态   → Anthropic 发起，2025-12 开放标准，多工具支持
└─ 安全   → 可信来源 + 审计脚本 + 警惕外部 URL
```

---

## 第二部分 · 实战篇：一个技能包的解剖

本仓库中的 `agent-skill-tutor/` 就是按上述规范搭建的真实技能包，可作为"活教材"对照理解。

### 2.1 目录结构

```
agent-skill-tutor/
├── SKILL.md                      # 技能定义：frontmatter（name + description）+ 教学流程
├── references/
│   ├── lesson.md                 # 图文教程（每节标注权威来源）
│   └── sources.md                # 权威来源清单（名称、链接、用途）
└── assets/
    └── adaptive-quiz.html        # 分层自适应测验（L1→L3，答对升级、答错降级）
```

### 2.2 结构对照解读

| 技能包元素 | 对应规范 | 本技能包的实现 |
|------------|----------|----------------|
| `SKILL.md` | 必需，含 name + description | 定义"教 Agent Skill"的触发条件与教学三步流程 |
| `references/` | 可选，按需加载的文档 | 教程与来源清单，避免撑大 SKILL.md |
| `assets/` | 可选，输出用文件 | 可交互的测验页面，直接给学习者使用 |

### 2.3 这个技能包如何被触发

1. 学习者说"我想学 Agent Skill"或"来个技能测验"；
2. Agent 比对 `SKILL.md` 的 description，命中后读取完整指令；
3. 按流程讲解概念（读 references/lesson.md）→ 展示实物 → 引导完成测验。

> 这正是"渐进式披露"的现场演示：描述先匹配，正文再加载，参考资料最后按需读取。

---

## 第三部分 · 测评篇：分层自适应测验（含答案与解析）

### 3.1 测验规则

- **三个层级**：L1 入门（基础概念）→ L2 进阶（规范与机制）→ L3 实战（开放标准与安全）；
- **答对升级**：同一层**连续答对 2 题**，自动升入下一层；
- **答错降级**：答错立即降回上一层（L1 答错不降，仅重新计数）；
- **通关条件**：在 L3 连续答对 2 题，即达到"精通"。

> 交互版见 `agent-skill-tutor/assets/adaptive-quiz.html`；以下为题库与答案解析的完整汇总。

### 3.2 L1 入门层（基础概念）

**L1-1. Agent Skill（智能体技能）最准确的定义是？**
A. 一种只能用于大模型训练的算法
B. 一个包含 SKILL.md 的文件夹，打包指令、脚本与资源，按需加载
C. 一段写死的自动化批处理代码
D. 一种新的深度学习模型架构

> ✅ **答案：B**
> 解析：技能是「文件夹 + SKILL.md 说明书」，可由 agent 动态发现并按需加载。
> 来源：Anthropic 官方文档

**L1-2. Skill 要解决的核心问题是什么？**
A. 让 AI 变快
B. 减少服务器成本
C. AI 虽聪明但缺少真实任务所需的过程性知识与领域上下文
D. 替代所有编程

> ✅ **答案：C**
> 解析：官方博客指出，agent 需要过程性知识（procedural knowledge）才能做好真实工作。
> 来源：Claude 官方博客

**L1-3. 一个技能包中唯一必需的文件是？**
A. README.md　B. config.json　C. SKILL.md　D. requirements.txt

> ✅ **答案：C**
> 解析：SKILL.md 是技能入口，frontmatter 含 name/description，正文为操作指令。
> 来源：Anthropic 官方文档 / agentskills.io

**L1-4. 技能包中 scripts/ 目录的用途是？**
A. 存放文档
B. 存放可执行代码（如 Python 脚本）供 agent 运行
C. 存放图片
D. 存放用户密码

> ✅ **答案：B**
> 解析：scripts 放可执行代码；确定性强的操作（如解析 PDF）用代码比模型生成更可靠。
> 来源：Anthropic 官方文档

**L1-5. 把 Skill 类比成什么最贴切？**
A. 给新员工的岗位操作手册　B. 硬件驱动程序　C. 数据库索引　D. 搜索引擎

> ✅ **答案：A**
> 解析：官方博客原文——构建技能好比给新人写入职引导（onboarding guide）。
> 来源：Claude 官方博客

### 3.3 L2 进阶层（规范与机制）

**L2-1. SKILL.md 的 YAML frontmatter 中必填的两个字段是？**
A. author 和 version　B. name 和 description　C. title 和 date　D. id 和 tags

> ✅ **答案：B**
> 解析：name 与 description 必填；description 决定 agent 何时触发技能。
> 来源：Anthropic 官方文档

**L2-2. 关于技能 name 字段的命名规则，正确的是？**
A. 可用大写和空格
B. 只能小写字母、数字与连字符，且 ≤64 字符
C. 必须含下划线
D. 可以叫 anthropic 或 claude

> ✅ **答案：B**
> 解析：name 须匹配目录名，不能含保留字 anthropic/claude，也不能含 XML 标签。
> 来源：Anthropic 官方文档

**L2-3.「渐进式披露」（Progressive Disclosure）的第一层是？**
A. 加载全部脚本　B. 只加载技能 name 与 description　C. 读取 references 全部内容　D. 执行所有示例

> ✅ **答案：B**
> 解析：启动时只把每个技能的 name + description 放进上下文，命中后再读全文。
> 来源：Claude 官方博客

**L2-4. agent 通常如何决定激活某个技能？**
A. 随机挑选
B. 任务与技能 description 描述的场景匹配时
C. 按字母顺序
D. 用户必须输入技能编号

> ✅ **答案：B**
> 解析：description 写明「做什么 + 何时用」，agent 据此自动匹配并加载 SKILL.md。
> 来源：Anthropic 官方文档 / agentskills.io

**L2-5. 什么信号提示你「该把内容沉淀成 Skill」了？**
A. 代码超过 100 行
B. 反复把同一套流程粘贴进对话，或说明文件膨胀成操作步骤
C. 文件太多
D. AI 回答太慢

> ✅ **答案：B**
> 解析：Claude Code 文档——当重复粘贴剧本/清单，或 CLAUDE.md 长成流程而非事实时，就建 skill。
> 来源：Claude Code 文档

### 3.4 L3 实战层（开放标准与安全）

**L3-1. Agent Skills 是何时被正式发布为「开放标准」的？**
A. 2024 年 6 月　B. 2025 年 12 月　C. 2023 年 1 月　D. 2026 年 8 月

> ✅ **答案：B**
> 解析：Anthropic 2025-10 推出，2025-12-18 发布为开放标准（agentskills.io）。
> 来源：Claude 官方博客 / agentskills.io

**L3-2. description 字段对内容的要求是？**
A. 越短越好，只写关键词
B. 必须同时说明技能做什么以及何时使用，≤1024 字符
C. 必须写满 1024 字
D. 只能写英文

> ✅ **答案：B**
> 解析：description 是技能最重要的字段——既写功能也写触发场景，才能被正确激活。
> 来源：Anthropic 官方文档

**L3-3. 关于 Skill 与 MCP 的关系，正确的是？**
A. 两者完全相同
B. MCP 管「怎么连工具和数据」，Skill 管「一件事该怎么做」，互补
C. Skill 是 MCP 的替代品
D. MCP 必须依赖 Skill 才能运行

> ✅ **答案：B**
> 解析：业界通行理解——MCP 给 agent「手」，Skill 给 agent「方法/流程」，二者互补。
> 来源：agentskills.io / Anthropic 示例技能库

**L3-4. 使用第三方 Skill 前的安全审计重点包括？**
A. 只看名字是否好听
B. 审查捆绑文件是否有意外网络请求、异常文件访问或与描述不符的操作
C. 确认图标好看
D. 无需任何检查

> ✅ **答案：B**
> 解析：官方安全章节——审计 SKILL.md 与脚本；外部 URL 拉取内容的技能风险尤其高。
> 来源：Anthropic 官方文档

**L3-5. references/ 目录在技能中的作用是？**
A. 只在需要时被读取的参考资料（如流程指南、API 文档）
B. 启动必加载的配置
C. 存放二进制文件
D. 存放许可证

> ✅ **答案：A**
> 解析：references 属第三层披露——SKILL.md 保持精简，细节资料按需读取，节省上下文。
> 来源：Anthropic 官方文档

### 3.5 测评结果记录表

| 层级 | 题目数 | 我的得分 | 掌握状态 |
|------|--------|----------|----------|
| L1 入门 | 5 | ／5 | ☐ 已掌握 |
| L2 进阶 | 5 | ／5 | ☐ 已掌握 |
| L3 实战 | 5 | ／5 | ☐ 已掌握 |
| **通关判定** | 15 | —— | ☐ 达成"精通" |

---

## 第四部分 · 成果篇：学习数据看板

### 4.1 三色可视化说明

本仓库使用**三种颜色**统一呈现学习数据，含义如下：

| 颜色 | 含义 | 用于 |
|------|------|------|
| 🔵 蓝 `#2563EB` | 信息与结构 | 文档、知识点、未开始 |
| 🟠 橙 `#F59E0B` | 进行中 | 当前阶段、待办事项 |
| 🟢 绿 `#10B981` | 已完成 | 已达成目标、已完成提交 |

> 交互式看板见仓库根目录 `dashboard.html`（用浏览器打开）。

### 4.2 学习数据统计

| 指标 | 数值 | 说明 |
|------|------|------|
| 仓库文件总数 | 14 | 笔记 / 代码 / 技能包 / 文档 |
| Git 提交次数 | 7 | 每次提交 = 一次学习存档 |
| 学期进度 | 1 / 14 周 | 已完成第 1 周 |
| 技能包资源 | 3 类 | SKILL.md + references + assets |

### 4.3 仓库内容构成

| 目录 | 文件数 | 用途 |
|------|--------|------|
| `notes/` | 4 | 每周学习笔记 |
| `agent-skill-tutor/` | 4 | 自建技能包（本汇总的主角） |
| `data/` | 2 | 数据集说明 |
| 根目录文档 | 2 | README、.gitignore |
| `code/` | 1 | 代码练习 |
| `dist/` | 1 | 技能包打包产物 |

### 4.4 学习里程碑

| 日期 | 里程碑 | 状态 |
|------|--------|------|
| 2026-09-03 | 环境搭建（Git / Python 3.12 / VSCode）+ 注册 GitHub | 🟢 已完成 |
| 2026-09-03 | 创建仓库并跑通 clone → add → commit → push | 🟢 已完成 |
| 2026-09-03 | 自建 Agent Skill 技能包（教程 + 分层测验） | 🟢 已完成 |
| 2026-09-06 | 改造为学习主页（README 导航 + 笔记模板 + 进度表） | 🟢 已完成 |
| 2026-09-10 | 作业规范化 + 三色数据看板 | 🟢 已完成 |

---

## 附录 A · 权威来源清单

| # | 来源名称 | 链接 |
|---|----------|------|
| 1 | Anthropic 官方文档《Agent Skills Overview》（英文） | https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview |
| 2 | Anthropic 官方文档《Agent Skills》（中文） | https://console.anthropic.com/docs/zh-CN/agents-and-tools/agent-skills/overview |
| 3 | Claude Code 文档《使用 skills 扩展 Claude》 | https://code.claude.com/docs/zh-TW/skills |
| 4 | Claude 官方博客《Equipping agents for the real world with Agent Skills》 | https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills |
| 5 | Agent Skills 开放标准官网 | https://agentskills.io/ |
| 6 | Agent Skills 规范页 | https://agentskills.io/specification |
| 7 | Anthropic 开源示例技能库 | https://github.com/anthropics/skills |

> 以上链接核验日期：2026-09-03。若链接失效，请以官方网站最新文档为准。

---

## 附录 B · 术语表

| 术语 | 英文 | 含义 |
|------|------|------|
| 智能体 | Agent | 能理解目标、自主规划、调用工具完成任务的 AI 系统 |
| 技能 | Skill | 打包专业流程与资源的可复用文件夹（含 SKILL.md） |
| 渐进式披露 | Progressive Disclosure | 三层按需加载机制，先描述后正文再资源 |
| 元信息 | Frontmatter | SKILL.md 顶部的 YAML 字段，含 name 与 description |
| 过程性知识 | Procedural Knowledge | "怎么做一件事"的操作性知识，区别于事实性知识 |

---

*本文档由 Mom-Hue 整理，用于《大数据与人工智能》课程学习记录。*
*合并来源：`agent-skill-tutor/references/lesson.md`、`agent-skill-tutor/references/sources.md`、`agent-skill-tutor/assets/adaptive-quiz.html`、`dashboard.html`。*
