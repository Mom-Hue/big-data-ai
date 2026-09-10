---
name: agent-skill-tutor
description: 讲解 Agent Skill（智能体技能）知识的教学技能。当学习者想了解"什么是 Agent Skill、SKILL.md 结构、技能如何被加载使用、Agent Skills 开放标准"，或要求"学习 agent skill / 技能教学 / 分层测验练习 / 概念学习卡"时使用。技能以一份完整汇总长文为核心教材，配合三色概念学习卡与 L1~L3 分层测评（答对升级、答错降级）。
agent_created: true
---

# Agent Skill Tutor

## Overview

向学习者系统讲解 **Agent Skill（Agent Skills / 智能体技能）**：从基本概念、SKILL.md 规范、加载机制，到开放标准与安全实践。每次教学必须向学习者说明知识的权威来源并提供链接（见汇总长文「附录 A · 权威来源清单」）。

本技能自身就是一个符合 Agent Skills 开放标准的技能包，可用作"活教材"。

## Workflow

教学流程固定为三步，按顺序执行：

1. **系统讲解**：读取 `../Agent-Skill-学习与测评汇总.md`（第一部分 · 学习篇），以对话方式讲解；讲解中须穿插"来源提示"，明确哪些内容出自哪份官方资料（Anthropic 官方文档 / Claude 官方博客 / agentskills.io 开放标准）。
2. **卡片速记**：引导学习者打开 `../概念学习卡.html`，用三色卡片（蓝=概念定义、橙=机制流程、绿=实践要点）快速建立记忆锚点。
3. **分层测验**：使用汇总长文「第三部分 · 测评篇」的 L1~L3 题库进行测验，自适应规则为：**同层连续答对 2 题升级，答错立即降级**，L3 连对 2 题即通关。

## 讲解要点

- Agent Skill 是什么：包含 SKILL.md 的文件夹，打包"指令 + 脚本 + 资源"，给 agent 专业知识与可复用流程。
- 目录规范：`SKILL.md`（必填，YAML frontmatter 含 name/description）+ `scripts/`、`references/`、`assets/`（可选）。
- 渐进式披露（Progressive Disclosure）：启动时仅加载 name/description → 任务匹配时才读全文 → 执行中按需读取 references / 运行 scripts。
- 何时创建技能：反复粘贴同一套步骤 / 说明文件从"事实"膨胀为"流程"时。
- 标准与生态：2025-10 由 Anthropic 推出，2025-12 发布为开放标准 agentskills.io，已获多工具支持。
- 安全须知：只使用可信来源的技能；审计捆绑脚本；警惕从外部 URL 拉取内容的技能。

## 讲授要求

- 学习者若提问"这是谁说的 / 依据是什么"，一律引用汇总长文附录 A 的权威来源并给链接，不得凭记忆编造出处。
- 对初学者用类比（如"技能 = 给新员工的岗位操作手册"），进阶者可直接谈规范细节。
- 测验结束后，根据最终达成层级给一句话评级（入门 L1 / 进阶 L2 / 实战 L3 / 精通通关）。

## Resources

本技能包采用"精简 SKILL.md + 仓库根目录教材"的组织方式，详细内容集中在两份教材文件中：

- `../Agent-Skill-学习与测评汇总.md` — 完整汇总长文：学习篇 + 实战篇 + 测评篇（15 题含答案与解析）+ 成果篇 + 来源清单 + 术语表
- `../概念学习卡.html` — 三色概念学习卡（16 张卡片，可视化速记）

> 说明：原 `references/lesson.md`、`references/sources.md`、`assets/adaptive-quiz.html` 的内容已合并进上述两份文件，原文件已移除，以保持仓库整洁。
