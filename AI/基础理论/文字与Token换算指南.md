---
title: 文字与 Token 换算指南
created_at: 2026-02-05
updated_at: 2026-02-05
tags: [Token, Tokenizer, 计费优化, LLM基础]
---

# 文字与 Token 换算指南

## 1. 快速估算公式
- **英文**: `Word Count / 0.75 = Tokens`
- **中文 (现代国产模型如 DeepSeek/Qwen)**: `汉字数 * 1.0 ≈ Tokens`
- **中文 (早期 GPT 模型)**: `汉字数 * 2.0 ≈ Tokens`

## 2. 影响 Token 数量的因素
1. **词表大小 (Vocab Size)**: 词表越大，模型一次能“读”进去的内容就越多（单 Token 包含的信息量大），Token 消耗就越低。
2. **语言编码**: UTF-8 编码下，汉字占 3 字节。如果分词器优化不好，会按字节计费，导致中文极贵。
3. **格式化符号**: 
   - 连续空格、制表符（Tab）在代码生成中会消耗不少 Token。
   - JSON 格式的 Key-Value 结构比纯文本更费 Token。

## 3. 计费建议
- **合并请求**: 避免频繁的小请求，因为每个请求都有 Prompt 消耗（System Prompt + History）。
- **精简上下文**: 定期清理对话历史，或者使用摘要技术（Summarization）减少 Context 长度。
- **选择合适的模型**: 处理长中文建议使用针对中文优化的分词器。

## 参考工具
- [Tiktoken (OpenAI)](https://github.com/openai/tiktoken)
- [Hugging Face Tokenizers](https://github.com/huggingface/tokenizers)
