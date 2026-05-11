---
title: Few-shot 提示词技巧：给模型的“参考范例”
created_at: 2026-05-11
updated_at: 2026-05-11
tags: [Prompt-Engineering, Few-shot, In-context-Learning]
---

# Few-shot 提示词技巧

## 1. 定义
**Few-shot (少样本提示)** 是指在 Prompt 中包含一个或多个输入输出对（范例），利用模型的 **In-context Learning (上下文学习)** 能力，引导模型理解任务目标、输出格式或特定风格。

## 2. 核心价值
- **格式对齐**：强制模型按 JSON、Markdown 或特定符号输出。
- **逻辑引导**：展示复杂的推理步骤，减少模型“幻觉”。
- **风格迁移**：让模型模仿特定的语气或排版。

## 3. 实战建议
- **质量高于数量**：通常 3-5 个高质量示例优于 10 个平庸示例。
- **多样性**：示例应覆盖不同的边界情况。
- **顺序固定 (关键)**：为了最大化 [[Prompt-Caching-优化指南]]，示例的顺序必须在请求间保持一致。

## 4. 与其他技术的结合
- **CoT (Chain of Thought)**：在 Few-shot 示例中加入思考过程。
- **Self-Consistency**：提供多个示例后让模型多次生成并取多数票。

## 5. Update History
- 2026-05-11: 初次创建。
