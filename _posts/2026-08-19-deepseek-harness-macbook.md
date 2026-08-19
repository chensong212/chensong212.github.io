---
layout:     post
title:      DeepSeek Harness 本地部署：从踩坑到跑通
subtitle:   科研记录 | MacBook + Ollama + qwen3 本地 Agent 部署全流程
date:       2026-08-19
author:     陈陈
header-img: img/post-bg-coffee.jpeg
catalog: true
category: 科研记录
tags:
    - 科研笔记
    - 编程相关
    - 经验总结
---

## 问题背景

2026 年 8 月 13 日，DeepSeek 正式开源了 **DeepSeek Harness (DSH)** 开发者预览版。这是一个 Agent 运行框架，能让大模型从"只会聊天"变成"能自主干活"的智能体——自动读写文件、执行终端命令、分析代码、修复 Bug。

但 DeepSeek 官方的 Harness 默认只接 DeepSeek 自己的 API（需要 API Key 和额外充值）。实际上，Harness 的插件架构（"一切皆插件"）允许接入任何 OpenAI 兼容的模型服务，包括本地 Ollama。

本文记录在 **MacBook Air M5 (32GB)** 上，从零开始部署 DeepSeek Harness、连接本地 Ollama qwen3 模型，并解决踩坑问题的完整过程。所有命令可直接复制粘贴到终端执行。

---

## 环境准备

| 组件 | 版本/型号 | 说明 |
|---|---|---|
| 操作系统 | macOS (Apple Silicon) | M5 芯片 |
| 内存 | 32GB 统一内存 | 可运行 14B 量化模型 |
| Ollama | 已安装 | 本地推理引擎 |
| 模型 | qwen3:14b / qwen3:8b | 已通过 `ollama run` 测试 |
| Node.js | 26.7.0 | Harness 运行环境 |

---

## 第一步：安装 Node.js

DeepSeek Harness 基于 Node.js，推荐 v20 以上版本。

```bash
# 通过 Homebrew 安装（macOS 推荐方式）
brew install node

# 验证安装
node --version   # 应输出 v26.x.x 或更高
npm --version    # 应输出 11.x.x 或更高
```

如果尚未安装 Homebrew，先执行：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---

## 第二步：全局安装 DeepSeek Harness

```bash
# 全局安装（需允许原生模块编译脚本）
npm install -g --allow-scripts=@deepseek-ai/dsh-subprocess-local,koffi,node-pty,@google/genai,protobufjs @deepseek-ai/dsh

# 验证安装
dsh --help
```

安装成功后，`dsh` 命令可用。关键子命令：

| 命令 | 用途 |
|---|---|
| `dsh web` | 启动 Web UI（浏览器交互） |
| `dsh --profile headless "任务"` | 命令行模式，执行一次任务 |
| `dsh plugin` | 管理插件 |

---

## 第三步：配置 Ollama 为模型提供商

这是最关键的一步。Harness 默认只接 DeepSeek 官方 API，需要通过 `~/.dsh/settings.yaml` 配置文件添加自定义模型提供商。

```bash
# 创建或编辑配置文件
cat > ~/.dsh/settings.yaml << 'EOF'
ui-onboarding:
  welcomeNoticeVersion: 2026-08-13.1

llm-pi-ai:
  providers:
    ollama-qwen3:
      displayName: "Ollama (本地 Qwen3)"
      api: "openai-completions"
      apiKeyEnv: "OLLAMA_API_KEY"
      baseURL: "http://localhost:11434/v1"
      models:
        - id: "qwen3:14b"
          name: "Qwen3 14B (主力)"
          contextWindow: 131072
        - id: "qwen3:8b"
          name: "Qwen3 8B (轻量)"
          contextWindow: 131072
EOF
```

**配置说明**：

| 字段 | 含义 | 注意事项 |
|---|---|---|
| `api: "openai-completions"` | 使用 OpenAI 兼容协议 | Ollama 的 `/v1` 端点兼容此协议 |
| `baseURL` | Ollama 服务地址 | 默认 `http://localhost:11434/v1` |
| `apiKeyEnv` | API Key 环境变量名 | **必填**，即使 Ollama 不需要认证 |
| `models[].id` | 模型标识 | 必须与 `ollama list` 输出一致 |

---

## 第四步：设置 API Key 环境变量（解决 PI_AI_ERROR）

启动 Harness 后，如果遇到以下错误：

```
No API key for provider: ollama-qwen3
PI_AI_ERROR
```

这是因为 Harness 的 pi-ai 适配器要求每个 provider 都有 API Key 凭证。虽然 Ollama 本地服务不需要认证，但 Harness 仍需要一个占位变量。

**解决方法**：

```bash
# 设置占位 API Key（Ollama 会忽略这个值）
export OLLAMA_API_KEY=ollama

# 建议写入 shell 配置文件，永久生效
echo 'export OLLAMA_API_KEY=ollama' >> ~/.zshrc
```

> **踩坑记录**：尝试过不设置 `apiKeyEnv` 字段，但 Harness 会直接报 `PI_AI_ERROR`。必须在 `settings.yaml` 中声明 `apiKeyEnv` 并在环境中导出对应变量，即使值为任意字符串。

---

## 第五步：启动 Harness 并验证

```bash
# 确保 Ollama 在运行
ollama list

# 启动 DeepSeek Harness Web 界面
dsh web
```

终端会输出类似：

```
Local:   http://localhost:5173/
```

用浏览器打开该地址，在模型选择下拉菜单中应能看到 **"Ollama (本地 Qwen3)"**，展开后有两个模型可选：

- **Qwen3 14B (主力)** — 适合编程、代码审查、论文审阅
- **Qwen3 8B (轻量)** — 适合翻译、润色、快速问答

---

## 第六步：体验 Agent 能力

在 Harness 对话框中可以尝试以下 Agent 任务：

### 测试 1：文件系统操作

```
列出当前工作目录下的所有文件，按大小排序，并告诉我最大的三个文件是什么
```

Harness 会自动调用 `ls`、`stat` 等工具，分析结果并给出回答。

### 测试 2：代码分析

```
分析当前目录下所有 Python 文件，找出潜在的代码问题并给出改进建议
```

Harness 会读取文件内容、调用代码分析工具，生成诊断报告。

### 测试 3：终端命令执行

```
检查系统当前的内存使用情况，并用中文总结关键指标
```

Harness 会执行 `vm_stat` 或 `top` 命令，解析输出并给出中文总结。

---

## 整体架构

```
┌──────────────────────────────────────────────────┐
│              DeepSeek Harness Web UI              │
│              http://localhost:5173                │
│                                                   │
│  ┌─────────────────────────────────────────────┐  │
│  │  Agent 核心能力（插件化）                      │  │
│  │  · 文件读写 (dsh-tool-fs)                     │  │
│  │  · Shell 执行 (dsh-tool-bash)                 │  │
│  │  · 代码编辑器 (dsh-tool-str-replace-editor)   │  │
│  │  · 子 Agent (dsh-tool-subagent)               │  │
│  │  · 联网搜索 (dsh-tool-web)                    │  │
│  └──────────────────┬──────────────────────────┘  │
│                     │                              │
│            ~/.dsh/settings.yaml                    │
│          llm-pi-ai → ollama-qwen3                  │
│                     │                              │
└─────────────────────┼──────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────┐
│              Ollama (本地推理引擎)                 │
│           http://localhost:11434/v1               │
│                                                   │
│  ┌─────────────────┐  ┌─────────────────────────┐ │
│  │  qwen3:14b (9GB) │  │  qwen3:8b (5GB)         │ │
│  │  主力：编程/推理  │  │  轻量：翻译/润色         │ │
│  └─────────────────┘  └─────────────────────────┘ │
└──────────────────────────────────────────────────┘
```

---

## 常见问题排查

| 问题 | 原因 | 解决方法 |
|---|---|---|
| `No API key for provider` | 未设置 `apiKeyEnv` 或环境变量 | `export OLLAMA_API_KEY=ollama` |
| 模型选择中没有 Ollama | 配置未生效 | 重启 `dsh web`，检查 `settings.yaml` 语法 |
| `ECONNREFUSED` 连接拒绝 | Ollama 未启动 | 先执行 `ollama serve` 或确认 Ollama 在运行 |
| 模型名称不匹配 | settings.yaml 中 id 与 `ollama list` 不一致 | 确保 `models[].id` 与 `ollama list` 的 NAME 完全一致 |
| npm 安装脚本未执行 | 未允许 install scripts | 使用 `--allow-scripts=` 参数重新安装 |

---

## 关键结论

1. **DeepSeek Harness 可以在 MacBook 上完全本地运行**，不需要 DeepSeek API Key，也不需要额外充值。
2. **接入 Ollama 的核心是 `~/.dsh/settings.yaml` 文件**，通过 `llm-pi-ai` 命名空间下的 `providers` 字典配置自定义模型。
3. **`apiKeyEnv` 是必填字段**，即使目标服务不需要认证（如 Ollama），也必须设置一个占位环境变量，否则会触发 `PI_AI_ERROR`。
4. **qwen3:14b 作为 Agent 推理后端表现良好**，在 32GB MacBook 上可以流畅运行文件读写、代码分析、Shell 执行等 Agent 任务。
5. 这套方案是实验室 AI 架构的**本地轻量层**——重型任务（DeepSeek-Coder-33B / V3 等大模型）可以部署在 SCNet 超算 GPU 集群上，通过同样的 Harness 配置接入。

---

## 参考资料

- DeepSeek Harness GitHub: [https://github.com/deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness)
- Ollama 官方文档: [https://ollama.com](https://ollama.com)
- DeepSeek Harness 发布公告: 2026 年 8 月 13 日，MIT 协议开源