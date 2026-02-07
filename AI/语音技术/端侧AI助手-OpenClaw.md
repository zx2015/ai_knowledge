---
title: 本地 AI 助手：Clawdbot / Moltbot (OpenClaw) 详解
created_at: 2026-02-05
updated_at: 2026-02-05
tags: [AI-Agent, 开源工具, 本地AI, 自动化]
---

# 本地 AI 助手：Clawdbot / Moltbot (OpenClaw)

## 1. 项目背景与更名缘由
- **Clawdbot**：项目初创名称，因商标争议被迫弃用。
- **Moltbot**：取自“脱壳（Molt）”，寓意蜕变与重生。
- **OpenClaw**：目前的正式开源项目名称。

## 2. 核心功能
不同于普通的聊天机器人（Chatbot），它是一个 **AI Agent（智能体）**：
- **执行能力**：具备“手脚”，可以操作文件系统、执行 Shell 脚本。
- **消息接入口**：支持 WhatsApp, Telegram, Discord, Slack 等主流 IM 工具。
- **主动性**：可以根据预设逻辑或用户习惯，主动介入工作流（如整理每日简报、自动回信）。

## 3. 技术架构
- **模型支持**：支持所有兼容 OpenAI 格式的 API（如 GPT-4o, Claude 3.5 Sonnet）以及本地模型（Ollama/Llama 3）。
- **隐私保护**：核心逻辑在本地运行，用户可以控制数据的流向。
- **开源协议**：MIT 协议。

## 4. 行业意义
该项目的火爆代表了 **“端侧 AI（On-device AI）”** 的一个新方向：AI 不再只是网页上的一个对话框，而是无缝嵌入到用户现有的通信工具和操作系统中，成为真正的个人助理。

## 参考链接
- [Clawdbot (OpenClaw) GitHub](https://github.com/petersteinberger/OpenClaw)
- [Moltbot Official Site](https://moltbot.com/)
