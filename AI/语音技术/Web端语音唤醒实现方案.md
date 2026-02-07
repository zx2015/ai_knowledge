---
title: Web 端数字人语音唤醒实现方案
created_at: 2026-02-07
updated_at: 2026-02-07
tags: [WebAudio, WebAssembly, 数字人, KWS]
---

# Web 端数字人语音唤醒实现方案

在 Web 数字人场景中，为了保证响应速度和降低服务器压力，通常采用 **“前端本地唤醒 + 后端业务处理”** 的架构。

## 1. 核心技术栈

### 1.1 WebAssembly (Wasm)
Wasm 允许在浏览器中以接近原生的速度运行 C++/Rust 编写的 KWS 引擎。
- **Sherpa-ONNX (Wasm)**: 支持离线运行 Zipformer 等先进模型。
- **TensorFlow.js**: 也可以训练简单的分类模型，但性能和准确度通常不如专用 KWS 引擎。

### 1.2 Web Audio API & AudioWorklet
- **AudioWorklet**: 它是 Web Audio API 的一部分，允许在独立于主线程的渲染线程中进行音频处理，防止音频卡顿或由于 JS 主线程阻塞导致的漏唤醒。

## 2. 推荐实施方案

### 方案 A：Sherpa-ONNX Wasm (最推荐)
- **特点**: 完全开源，支持中文，模型文件可量化压缩至 3MB 左右。
- **流程**:
    1. 加载 `sherpa-onnx-wasm`。
    2. 在 `AudioWorkletProcessor` 中接收麦克风采样。
    3. 调用 Wasm 接口进行实时推理。
    4. 触发事件通知前端开启数字人交互。

### 方案 B：Picovoice Porcupine (高性能/商业)
- **特点**: 准确率极高，误报率极低，支持 50+ 种语言。
- **优势**: 提供专门的 Web SDK，几分钟内即可完成集成。

## 3. 部署形态：网页 (Web App) vs 插件 (Extension)

### 3.1 网页形态 (Web App)
- **场景**: 独立数字人交互网站。
- **优点**: 无需安装，跨平台性好。
- **限制**: 必须保持标签页开启；受限于浏览器对后台标签页的资源调度限制。

### 3.2 插件形态 (Extension)
- **场景**: 全局 AI 助手，在任意网页下唤醒。
- **优点**: 可以利用 `Side Panel` 或 `Offscreen Documents` 保持更稳定的后台监听；支持跨页面 UI 覆盖。

## 4. 浏览器安全策略与限制

1.  **HTTPS 限制**: 麦克风权限（getUserMedia）仅限 HTTPS 或 localhost 环境。
2.  **用户首动触发 (User Gesture)**: 浏览器禁止页面在没有用户交互（点击、按键）的情况下自动开启麦克风。因此，数字人页面通常需要一个“开始通话”或“进入空间”的按钮。
3.  **后台挂起**: Chrome 会对不活跃的后台标签页进行“冻结”。若需后台持续唤醒，建议使用插件形态或利用 `Web Workers` 维持活跃。

## 5. 实现细节注意点

1.  **模型预加载**: Wasm 和模型文件通常有几 MB，应在页面加载时进行预取（Prefetch），避免用户想说话时还没加载完。
2.  **浏览器权限策略**: 浏览器要求必须有**用户交互（如点击页面任何地方）**后才能开启麦克风录音。
3.  **回声消除 (AEC)**: 数字人说话时，其声音会被麦克风录入导致“自唤醒”。需开启 Web Audio API 的 `echoCancellation` 配置。

## 4. 混合架构示例

| 组件 | 所在位置 | 负责任务 |
| :--- | :--- | :--- |
| **KWS 引擎** | 浏览器 (Wasm) | 监听关键词，触发交互 |
| **VAD (语音检测)** | 浏览器 (Wasm) | 判断人声起止，节省流量 |
| **ASR/NLP/TTS** | 远程服务器 | 核心逻辑处理 |
| **数字人渲染** | 浏览器 (Canvas/WebGL) | 视觉展现 |

## 参考链接
- [Sherpa-ONNX Web Demo](https://k2-fsa.github.io/sherpa/onnx/wasm/index.html)
- [MDN AudioWorklet API](https://developer.mozilla.org/en-US/docs/Web/API/AudioWorklet)
