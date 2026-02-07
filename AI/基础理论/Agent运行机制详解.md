---
title: AI Agent 运行机制与成本分析
created_at: 2026-02-05
updated_at: 2026-02-05
tags: [AI-Agent, LLM, 成本分析, ReAct]
---

# AI Agent 运行机制与成本分析

## 1. 核心工作流：ReAct 循环
Agent 的本质是 LLM 与外部工具（Tools）的闭环交互。一个标准的 Agent 步骤包含：
- **Thought (思考)**: LLM 分析当前状态。
- **Action (行动)**: LLM 决定调用哪个工具及参数。
- **Observation (观察)**: 获取工具执行后的反馈（如搜索结果、代码运行报错）。

## 2. LLM 调用次数构成
一个复杂的请求会被拆解为多次调用：
1. **Initial Planning**: 拆解目标。
2. **Tool Selection & Parameter Generation**: 每次使用工具都需要一次调用。
3. **Observation Processing**: 解读工具返回的海量信息。
4. **Error Recovery**: 工具失败时的重试逻辑。
5. **Final Synthesis**: 最终总结。

## 3. 性能与成本权衡
| 维度 | 普通 Chat | AI Agent |
| :--- | :--- | :--- |
| **调用次数** | 固定 1 次 | 动态 N 次 (通常 3-20) |
| **Token 消耗** | 较低 | 极高 (包含大量工具输出和历史轨迹) |
| **准确度** | 取决于模型预训练知识 | 能够通过实时数据校准，准确度更高 |
| **时延** | 毫秒/秒级 | 受限于循环次数，可能需要分钟级 |

## 4. 优化方向
- **提示词工程**: 减少冗余的思考步骤。详见：[[Prompt优化与Token减支策略]]。
- **模型微调**: 让模型更擅长一次性输出多个工具调用。
- **缓存技术**: 利用 Context Caching 减少重复前缀开销。
- **压缩算法**: 使用 LLMLingua 等方案压缩长上下文。

## Update History
- 2026-02-05: 初次创建，解析 Agent 处理请求时的 LLM 调用逻辑。
