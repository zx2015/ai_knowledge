---
title: Vibe Coding 实战指南：AI 驱动的意图编程技巧
created_at: 2026-05-12
updated_at: 2026-05-12
tags: [Vibe-Coding, Cursor, Windsurf, Karpathy, AI-Agent]
---

# Vibe Coding 实战指南：AI 驱动的意图编程技巧

**Vibe Coding** 是由 Andrej Karpathy 推广的一种新型开发范式：开发者不再纠结于语法细节，而是通过描述“意图（Intent）”和“感觉（Vibe）”，由 AI Agent（如 Cursor, Windsurf）负责具体的实现与调试。

## 1. 核心心法：先规格，后氛围 (Spec before Vibe)
Vibe Coding 并不等于“乱写”。高质量的输出依赖于高质量的逻辑输入：
- **PLANNING.md**：在开始前，先让 AI 生成一份逻辑规划文件，确认无误后再执行。
- **README 驱动**：先定义好产品的交互逻辑和运行方式，作为 AI 的“北极星指标”。

## 2. 工具链优化技巧

### 2.1 Cursor (The Composer Way)
- **`.cursorrules`**：在项目根目录定义全局编程规范（如：强制使用 Functional Programming、Tailwind CSS 等），这是减少 AI “幻觉”的最高效手段。
- **上下文精准索引 (`@`)**：通过 `@Files` 减少无关代码的干扰，通过 `@Docs` 让 AI 获取第三方库的最前沿版本信息。

### 2.2 Windsurf (The Flow Way)
- **Cascade 代理权限**：充分利用其自主运行终端命令和读取文件系统的能力。在 Windsurf 中，开发者的角色更多是“审计者”。

## 3. 实战沟通范式
- **原子化交互**：遵循“一个意图 -> 一次修改 -> 一次验证”的闭环。
- **语音输入 (High Throughput)**：人类口述逻辑的速度远超打字，使用语音工具描述复杂业务场景。
- **对比法**：利用 AI 的审美迁移能力，使用“风格类似 Apple/Stripe”等描述。

## 4. 风险控制与防御
- **诊断优先**：遇到错误，先让 AI 解释原因，再执行修复，防止无效的“盲目尝试”。
- **测试前置**：要求 AI 为所有核心逻辑编写自动化测试。**“没有测试的代码是不存在的。”**
- **Git 频繁回滚**：警惕“毁灭性循环（Doom Loop）”，即 AI 连续三次修复失败。此时应果断 `git reset --hard` 并重新梳理 Prompt。

## 5. Update History
- 2026-05-12: 初次创建，基于 Karpathy 的 Vibe Coding 理念整理。

## 相关链接
- [[Karpathy-LLM-Wiki-理念深度解析]]
- [[AI-Agent-架构与框架全景指南]]
