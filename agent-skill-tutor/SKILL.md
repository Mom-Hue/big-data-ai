---
name: agent-skill-tutor
description: 讲解 Agent Skill（智能体技能）知识的教学技能。当学习者想了解"什么是 Agent Skill、SKILL.md 结构、技能如何被加载使用、Agent Skills 开放标准"或要求"学习 agent skill / 技能教学 / 分层测验练习"时使用。技能内置带知识来源链接的图文教程与"答对升级、答错降级"的分层自适应测验。
agent_created: true
---

# Agent Skill Tutor

## Overview

向学习者系统讲解 **Agent Skill（Agent Skills / 智能体技能）**：从基本概念、SKILL.md 规范、加载机制，到开放标准与安全实践。每次教学必须向学习者说明知识的权威来源并提供链接（见 references/sources.md）。

本技能自身就是一个符合 Agent Skills 开放标准的技能包，可用作"活教材"。

## Workflow

教学流程固定为三步，按顺序执行：

1. **讲解概念**：读取 `references/lesson.md`，以对话方式讲解；讲解中须穿插"来源提示"，明确哪些内容出自哪份官方资料（Anthropic 官方文档 / Claude 官方博客 / agentskills.io 开放标准）。
2. **展示实物**：打开 `assets/adaptive-quiz.html` 让学习者看到本技能包的真实目录结构（SKILL.md + references/ + assets/），对照讲解。
3. **分层测验**：引导学习者打开 `assets/adaptive-quiz.html` 完成测验。测验为自适应模式：**答对连续两题升级，答错降级**（见 quiz 内部规则）。

## 讲解要点（对应 lesson.md 章节）

- Agent Skill 是什么：包含 SKILL.md 的文件夹，打包"指令 + 脚本 + 资源"，给 agent 专业知识与可复用流程。
- 目录规范：`SKILL.md`（必填，YAML frontmatter 含 name/description）+ `scripts/`、`references/`、`assets/`（可选）。
- 渐进式披露（Progressive Disclosure）：启动时仅加载 name/description → 任务匹配时才读全文 → 执行中按需读取 references / 运行 scripts。
- 何时创建技能：反复粘贴同一套步骤 / CLAUDE.md 从"事实"膨胀为"流程"时。
- 标准与生态：2025-10 由 Anthropic 推出，2025-12 发布为开放标准 agentskills.io，已获多工具支持。
- 安全须知：只使用可信来源的技能；审计捆绑脚本；警惕从外部 URL 拉取内容的技能。

## 讲授要求

- 学习者若提问"这是谁说的 / 依据是什么"，一律引用 sources.md 中的权威来源并给链接，不得凭记忆编造出处。
- 对初学者用类比（如"技能 = 给新员工的岗位操作手册"），进阶者可直接谈规范细节。
- 测验结束后，根据最终达成层级给一句话评级（见 assets/adaptive-quiz.html 通关文案）。

## Resources

### references/
- `lesson.md` — 主教程（图文 + 各章节来源标注）
- `sources.md` — 权威来源清单（名称、链接、用途）

### assets/
- `adaptive-quiz.html` — 分层自适应测验（L1 入门 → L2 进阶 → L3 实战；答对升级、答错降级，含解析与来源提示）
