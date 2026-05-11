---
title: Prompt Caching 优化指南：提升缓存命中率的实战策略
created_at: 2026-05-11
updated_at: 2026-05-11
tags: [Prompt-Engineering, Cache, Optimization, DeepSeek]
---

# Prompt Caching 优化指南

## 1. 核心原理：前缀匹配 (Prefix Matching)
几乎所有主流大模型的缓存机制都遵循**从第 0 个 Token 开始的连续匹配**。只要前缀完全一致，即可复用 KV Cache。

## 2. 黄金法则：静态前置，动态后置
将 Prompt 按照稳定性从高到低进行排序：

1. **System Prompt** (极高稳定性)
2. **知识库/参考文档** (高稳定性，尤其是长文本)
3. **Few-shot 示例** (中稳定性，确保顺序固定)
4. **任务指令模板** (低稳定性)
5. **用户当前问题** (动态，必须放最后)

## 3. 常见“缓存杀手”与对策
- **动态变量前置**：如在开头放时间、用户 ID、随机数。
    - *对策*：将变量移至 Prompt 末尾。
- **文本不规范**：额外的空格、换行或不同的转义符。
    - *对策*：在发送请求前进行标准化清理 (`strip()`)。
- **乱序检索结果**：RAG 场景下文档顺序随机。
    - *对策*：执行确定性排序（如按 ID 排序）。

## 4. DeepSeek 专项优化
- **单位阈值**：DeepSeek 缓存以 64 Tokens 为块。
- **计费优势**：命中缓存的价格通常仅为非命中的 10%。
- **监控**：实时记录并分析 API 返回的 `prompt_cache_hit_tokens`。

## 5. Update History
- 2026-05-11: 初次创建。
