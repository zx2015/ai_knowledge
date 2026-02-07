---
title: Qwen3-Coder-480B-A35B-Instruct 模型规格
created_at: 2026-02-05
updated_at: 2026-02-05
tags: [Qwen3, Coder-LLM, MoE, Context-Window]
---

# Qwen3-Coder-480B-A35B-Instruct 规格详解

## 1. 关键指标
- **上下文窗口 (Context Window)**: 
  - 原生支持 **256K Tokens**。
  - 支持通过扩展技术（如 YaRN）达到 **1M Tokens**。
- **总参数量**: 约 480B (480 Billion)。
- **激活参数量 (Active Parameters)**: 约 35B。
- **最大输出长度 (Max New Tokens)**: 依据配置，通常为 8K - 32K Tokens。

## 2. 架构特性
- **模型架构**: MoE (Mixture of Experts) 专家混合架构。
- **优化领域**: 针对编程任务（Code Generation, Debugging, Repo Understanding）深度优化。
- **推理效率**: 凭借 A35B 的激活规模，在同级别推理任务中具有极高的吞吐量和较低的延迟。

## 3. 为什么 256K/1M 的上下文很重要？
对于程序员而言，这意味着：
- **全项目感知**: 可以直接将整个 Git 仓库的代码输入模型。
- **文档长程依赖**: 能够同时阅读数份数千页的 API 文档而不会遗忘开头。
- **长代码生成**: 支持生成更完整的复杂系统代码模块。

## 参考链接
- [Qwen 官方博客](https://qwen.ai/)
- [Hugging Face - Qwen/Qwen3-Coder-480B-A35B-Instruct](https://huggingface.co/Qwen/)
