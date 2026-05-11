---
title: Prompt 优化与 Token 减支策略
created_at: 2026-02-06
updated_at: 2026-02-06
tags: [Prompt-Engineering, Token-Optimization, Context-Caching, LLMLingua]
---

# Prompt 优化与 Token 减支策略

在开发 AI Agent 时，基础 Prompt（系统指令、工具定义、Few-shot）往往占据了总 Token 消耗的 80% 以上。以下是业界主流的 Overhead 优化方案。

## 1. 基础设施优化：上下文缓存 (Context Caching)
这是目前最简单、最有效的降本方案。

| 技术 | 说明 | 优势 |
| :--- | :--- | :--- |
| **Prompt Cache** | 自动识别并缓存重复的 Prompt 前缀。 | 成本降低 50%-90%，TTFT 显著下降。 |
| **磁盘缓存技术** | 如 DeepSeek 默认开启的策略，将 KV Cache 存储在分布式硬盘阵列中。 | 自动运行，无需代码修改，价格极低。 |

**优化建议：**
- 将**静态内容**（System Message, Tool Definitions）置于 Prompt 顶部。
- 保持前缀一致性，避免在静态内容中间插入动态变量（如时间、随机 ID）。

## 2. 算法优化：提示词压缩 (Prompt Compression)
针对超长上下文，使用算法剔除冗余。

- **LLMLingua (微软)**：利用小模型评估 Token 重要性，实现 20 倍压缩。
- **LongLLMLingua**：专门针对长上下文中的关键信息提取，避免“迷失在中间（Lost in the middle）”现象。

## 3. 架构优化：Agentic Workflow
从单一的大提示词转向动态、模块化的架构。

- **动态 Prompt 加载**：通过意图识别，仅在必要时注入相关工具说明。
- **Multi-Agent 拆分**：将大任务拆给多个小 Agent，每个 Agent 拥有独立的、精简的 System Prompt。
- **结构化响应**：强制模型输出 JSON 或特定格式，减少闲聊（Chatter）产生的 Token 消耗。

## 4. 记忆管理策略
- **滑动窗口**：仅保留最近 N 轮对话。
- **语义总结**：使用 LLM 定期对旧对话进行摘要总结，用 100 Token 的总结代替 2000 Token 的原始记录。
- **外部记忆数据库**：结合 RAG 技术，将历史记忆存入向量数据库，仅在需要时检索相关的“记忆片段”放入 Prompt。

## 参考链接
- [DeepSeek Context Caching Guide](https://api-docs.deepseek.com/zh-cn/guides/kv_cache)
- [Microsoft LLMLingua Project](https://github.com/microsoft/LLMLingua)
- [OpenAI Prompt Caching documentation](https://platform.openai.com/docs/guides/prompt-caching)

## Update History
- 2026-02-06: 初次创建，总结了缓存、压缩、架构及记忆管理四类策略。
