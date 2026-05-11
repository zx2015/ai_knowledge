---
title: Gemini CLI Skills 配置指南
created_at: 2026-05-11
updated_at: 2026-05-11
tags: [Gemini-CLI, Skill, Configuration, Tooling]
---

# Gemini CLI Skills 配置指南

Skills 是扩展 Gemini CLI 能力的核心机制，允许 Agent 在需要时加载特定的专业领域知识或指令。

## 1. 存储层级 (Hierarchy)
Gemini CLI 遵循“局部优先”原则，按顺序从以下位置加载 Skill：
1. **项目级 (Workspace)**: `./.gemini/skills/` 或 `./.agents/skills/`
2. **全局级 (User)**: `~/.gemini/skills/` 或 `~/.agents/skills/`
3. **插件级 (Extension)**: 随安装的扩展包分发。

## 2. 定义规范 (SKILL.md)
每个 Skill 必须是一个独立的目录，且必须包含一个名为 `SKILL.md` 的文件。其核心是 YAML Frontmatter：

```markdown
---
name: <skill-name>
description: <描述模型在何种场景下应使用此技能，这是模型自动触发的关键>
---
# 指令与规范内容...
```

## 3. 运行机制
- **发现 (Discovery)**：启动会话时，CLI 扫描目录并将 `name` 和 `description` 注入系统提示词。
- **激活 (Activation)**：当用户需求匹配描述时，模型调用 `activate_skill` 工具，全量加载 `SKILL.md` 的内容。
- **状态控制**：通过 `settings.json` 中的 `skills.enabled` 和 `skills.disabled` 字段进行控制。

## 4. 管理命令
- `gemini skills list`: 列出可用技能。
- `gemini skills link <dir_path>`: 链接外部技能目录。
- `/skills`: 对话模式下查看技能。

## 5. Update History
- 2026-05-11: 初次创建。
