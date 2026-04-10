---
title: RAGAS 评估指标深度解析：Faithfulness, Relevancy 与 Robustness
created_at: 2026-04-10
updated_at: 2026-04-10
tags: [RAG, RAGAS, LLM-Evaluation, AI-Architecture]
---

# RAGAS 评估指标深度解析

RAGAS (Retrieval-Augmented Generation Assessment) 是目前行业内评估 RAG 系统最主流的框架。它采用 "LLM-as-a-Judge" 的思路，量化系统的可靠性。

## 1. 核心评估三元组 (The RAG Triad)

### 1.1 Faithfulness (忠实度)
*   **解决问题**：生成的回答是否在胡说八道（幻觉）？
*   **逻辑**：将 Answer 拆分为 Claims，验证每一个 Claim 是否能从 Context 中找到依据。

### 1.2 Answer Relevancy (回答相关性)
*   **解决问题**：模型是否在答非所问？
*   **逻辑**：根据生成的 Answer 反向推测用户可能问的问题，计算其与原始 Query 的相似度。

### 1.3 Context Precision (上下文精度)
*   **解决问题**：检索到的资料里有多少是干扰信息（信噪比）？
*   **逻辑**：评估排在前面的检索结果是否真的比后面的更有用。

## 2. 为什么需要 Robustness (鲁棒性)
在生产环境中，用户的问题可能包含陷阱，或者检索库中包含冲突信息。
*   **Negative Rejection**：如果资料里确实没有答案，模型是否敢于说不知道。
*   **Noise Sensitivity**：模型在嘈杂（充满无关干扰）的资料中提取核心事实的能力。

## 3. 评估流程示例
1.  **准备测试集**：包含 Query, Context, Answer。
2.  **调用 LLM 裁判**：通常使用 GPT-4 或 Qwen-Max 作为评分模型。
3.  **反馈闭环**：根据分数调整 Chunk Size、检索算法或 Prompt。

## Update History
- 2026-04-10: 初次创建，配合 RAG 优化指南。

## 相关链接
- [[RAG-Common-Issues-and-Optimization]]
- [[LLM微调技术详解]]
