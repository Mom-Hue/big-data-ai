# Agent Skill 图文教程

> 本教程所有关键知识点均标注了权威来源，来源全文与链接见同目录 `sources.md`。

---

## 1. 一句话定义

**Agent Skill（智能体技能）** 是一个"文件夹 + 说明书"形式的技能包：把某类任务的**专业流程、领域知识、脚本工具**打包成一个目录，让 AI 智能体在需要时自动发现并加载，从而把"通用助手"变成"领域专家"。

📖 来源：Anthropic 官方文档《Agent Skills》（见 sources.md #1）

---

## 2. 为什么需要它？（要解决的问题）

- AI 大模型虽然聪明，但**缺少"真实工作环境的过程性知识"**（procedural knowledge）与组织/业务上下文；
- 以前这些知识要么存在人的脑子里反复口头交代，要么写成超长提示词每次都全量加载；
- Skill 把知识**打包成可版本管理、可移植、按需加载**的文件，随取随用。

📖 来源：Claude 官方博客《Equipping agents for the real world with Agent Skills》（#3）

---

## 3. 技能包的标准结构

一个技能 = 一个目录，**至少包含一个 SKILL.md**：

```
skill-name/
├── SKILL.md              # 必需：YAML 元信息 + 操作指令
├── scripts/              # 可选：可执行代码（Python/Bash…）
├── references/           # 可选：按需加载的参考资料/文档
├── assets/               # 可选：输出用模板/图片/字体等
└── ...（可再扩展其他文件）
```

目录名 = 技能名 = SKILL.md 中 `name` 字段，三者一致。

📖 来源：agentskills.io 开放标准规范（#4）

---

## 4. SKILL.md 与 frontmatter 规范

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

> **description 是技能最重要的字段** —— 它决定了 agent 会不会在正确时机激活这个技能，要同时写清"做什么"与"何时用"。

📖 来源：Anthropic 官方文档 + agentskills.io 规范（#1 #4）

---

## 5. 核心机制：渐进式披露（Progressive Disclosure）

这是 Agent Skill 高效的关键，分三层按需加载：

| 层级 | 内容 | 何时进入上下文 |
|------|------|----------------|
| 第 1 层 | 技能 `name` + `description` | 会话启动即加载（仅约 100 tokens/技能） |
| 第 2 层 | SKILL.md 正文指令 | 任务与 description 匹配时才读取 |
| 第 3 层 | references/scripts/assets 等 | 执行中确有必要才加载/运行 |

好处：可以同时装几十个技能，上下文只承担"被用到的那几个"的开销。

📖 来源：Claude 官方博客 + Anthropic 官方文档（#3 #1）

---

## 6. 工作流：技能如何被使用

1. **发现（Discovery）**：启动时 agent 只读所有技能的 name/description；
2. **激活（Activation）**：当前任务与某个技能的 description 匹配 → agent 读取该技能完整 SKILL.md；
3. **执行（Execution）**：按指令操作，必要时运行 scripts 或读取 references。

你正在学的这个 `agent-skill-tutor` 本身就是个活教材——它就是按这套规范搭的。

📖 来源：agentskills.io 官方说明（#4）

---

## 7. 何时应该创建一个 Skill？

官方建议，出现以下信号就值得把内容沉淀为技能：

- 反复把**同一套脚本/清单/多步流程**粘贴进对话；
- 你的项目说明文件（如 CLAUDE.md）某部分已经从"事实"长成了"操作流程"；
- 希望把某领域的**专家做法固化**并分享给团队复用。

📖 来源：Claude Code 文档《使用 skills 扩展 Claude》（#2）

---

## 8. 生态与开放标准

- **2025-10**：Anthropic 在 Claude Code / Claude 应用中推出 Agent Skills；
- **2025-12-18**：发布为**开放标准**（open standard），官网 agentskills.io，参考实现与示例开源在 GitHub；
- 已被 Claude Code、OpenAI Codex、GitHub Copilot、Cursor、VS Code 等众多工具采纳，技能可跨产品复用。

> 与 MCP 的关系：MCP 管"智能体如何连上工具和数据"（给手），Skill 管"智能体该怎么做一件事"（给方法）。两者互补。

📖 来源：agentskills.io + shiplight 解读（#4 #5）

---

## 9. 安全须知（务必了解）

技能本质是"指令 + 可能被执行的代码"，因此：

- **只用可信来源**的技能（自己写的、官方或知名组织发布的）；
- 使用前**审计捆绑文件**（SKILL.md、脚本、图片）——警惕意外网络请求、异常文件访问、与描述不符的操作；
- **从外部 URL 拉取内容的技能风险更高**，其内容可能被篡改注入恶意指令；
- 涉及敏感数据/生产系统的技能要格外谨慎。

📖 来源：Anthropic 官方文档"安全注意事项"章节（#1）

---

## 10. 本课知识地图速览

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
