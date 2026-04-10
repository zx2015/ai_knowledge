---
title: RAG 系统常见问题与全链路优化指南
created_at: 2026-04-10
updated_at: 2026-04-10
tags: [RAG, 检索增强生成, LLM, 性能优化, 向量数据库]
---

# RAG 系统常见问题与全链路优化指南

RAG (Retrieval-Augmented Generation) 在落地过程中常面临“检索不准”和“生成幻觉”两大难题。本指南总结了从朴素 RAG 向高级/模块化 RAG 进阶的避坑经验。

## 1. 检索端优化 (The Retrieval Gap)

### 1.1 痛点：语义失准 (Semantic Mismatch)
*   **现象**：模型找回了一堆字面上相似但在逻辑上毫无关系的文档。
*   **对策**：
    *   **Hybrid Search (混合检索)**：向量相似度 (Dense) 负责捕捉语义，关键词 (Sparse/BM25) 负责捕捉专有名词。
    *   **BGE/Rerank**：使用专门的重排序模型对初步检索出的 100 条结果进行精选，保留 Top 5。

### 1.2 痛点：问题模糊 (Vague Queries)
*   **对策**：
    *   **HyDE (Hypothetical Document Embeddings)**：先让 LLM 生成一个“伪答案”，再用伪答案去检索。
    *   **Query Rewrite**：将多轮对话中的代词（如“它”）还原为具体的实体。

## 2. 生成端优化 (The Generation Gap)

### 2.1 痛点：幻觉与过度外推
*   **现象**：LLM 忽略了检索到的 Context，直接用它训练时的“固有知识”回答。
*   **对策**：
    *   **Strict Guardrails**：在 Prompt 中明确定义角色——“你是一个基于文档的助手，严禁脱离文档。如果文档没说，请诚实告知。”
    *   **Citation (引用溯源)**：要求模型在回答中标记出信息来源于哪篇文档的哪一行。

### 2.2 痛点：信息过载与 Lost in Middle
*   **对策**：
    *   **Context Filtering**：只喂给模型与问题相关性分数高于阈值的片段。
    *   **Long-Context Optimization**：如果必须处理极长文本，确保关键信息位于 Prompt 的开头和结尾（LLM 对中间位置最不敏感）。

## 3. 评估体系：RAGAS 三元组
评估是优化的前提。建议引入 [[RAGAS-Evaluation-Metrics]] 进行量化评分：
1.  **Faithfulness (忠实度)**：回答是否完全来自 Context？
2.  **Answer Relevancy (回答相关性)**：回答是否真的解决了问题？
3.  **Context Precision (检索精度)**：检索到的东西有多少是有用的？

## 4. 进阶架构：GraphRAG
对于需要跨文档关联的复杂咨询（如“对比 A 公司和 B 公司去年的研发投入”），传统的向量分块检索会失效。此时应考虑：
*   **实体提取**：将文档转化为节点和边。
*   **社区检测**：对相关知识点进行聚类。

## Update History
- 2026-04-10: 初次创建，涵盖检索、生成及评估全链路痛点分析。

## 相关链接
- [[RAGAS-Evaluation-Metrics]]
- [[开源向量数据库对比]]
- [[Prompt优化与Token减支策略]]
