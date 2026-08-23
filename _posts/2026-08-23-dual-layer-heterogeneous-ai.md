---
layout:     post
title:      构建双层异构 AI 协同网络
subtitle:   科研记录 | 基于 MacBook 本地轻量与 SCNet 云端 API 的学术 Agent 闭环方案
date:       2026-08-23
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

在高校实验室或前沿科研团队中，PI（项目负责人）和研究生每天需要处理大量的**文献分析、知识归纳、项目方案构建、论文评审、申请书撰写与修改**等任务。

传统的 AI 使用模式通常面临两难境地：
*   **本地部署**：隐私性高、零成本，但在轻量设备（如笔记本）上只能运行 14B 左右的中小模型，面对复杂的长文本综述和严密的逻辑推理时，能力明显受限。
*   **云端部署（自己租卡）**：虽然能跑大模型，但需要租用 GPU（如 4090/L20）、搭建容器、配置 PyTorch/vLLM 环境，且**按时间计费**（不管用不用，开着就扣费），算力预算白白浪费在排队和空转中。

为了打破这一僵局，我们通过本地 MacBook Air M5 与 SCNet 官方大模型 API，构建了一套**“本地轻量私密 + 云端按量重载”的双层异构 AI 协同网络架构**。

---

## 🏗️ 架构设计：双层异构 AI 协同网络

这套架构的核心是将**算力资产进行分层和自动路由**：

```
                    ┌──────────────────────────────────────────────┐
                    │            MacBook Air M5 (32GB)             │
                    │                                              │
                    │         【DeepSeek Harness Agent 平台】       │
                    └──────────────────────┬───────────────────────┘
                                           │
                    根据任务复杂度与类型，在 Harness 界面一秒切换：
                                           │
                     ┌─────────────────────┴─────────────────────┐
                     ▼                                           ▼
          【第一层：本地轻量私密】                     【第二层：云端重量级】
           Ollama 引擎（本地）                         SCNet 官方大模型 API（云端）
          qwen3:14b / qwen3:8b                         DeepSeek V4 Pro / Qwen3 30B
          
          · 日常轻量编写、代码自动补全                   · 重度文献归纳、申请书撰写与修改
          · 零网络延迟、完全不花钱                      · 逻辑深度推导、论文初稿评审
          · 离线安全                                   · 按 Token 扣除 5000 元额度，极度耐用
```

### 1. 本地轻量私密层（MacBook）
*   **硬件基础**：MacBook M5 芯片 + 32GB 统一内存，带宽极高。
*   **本地模型**：通过 Ollama 部署 `qwen3:14b`（主力）和 `qwen3:8b`（轻量）。
*   **应用场景**：编写日常小脚本、中英论文快速互译、行内代码自动补全（通过 VS Code Continue 插件）。

### 2. 云端重量级推理层（SCNet）
*   **后端支撑**：无须自行维护服务器，直接通过 SCNet 官方提供的 Token 计费大模型 API。
*   **云端模型**：
    *   `DeepSeek-V4-Pro`（深度推理/长文本）：用于学术论文评审、申请书纠错。
    *   `Qwen3-30B`（中文写作/学术）：用于方案大纲构建、PPT 和课件文本修改。
    *   `SCNet-Max`（平台旗舰）：用于复杂多步骤的自主 Agent 任务。
*   **计费模式**：**严格按 Token（实际生成字数）扣费**，5000 元的 Token 预算足够单人高频使用数年，性价比极高。

---

## 🛠️ 技术实现与踩坑记录

要将本地的 **DeepSeek Harness (DSH)** 接入 SCNet 云端官方 API，只需完成以下两步配置：

### 步骤一：配置本地 `settings.yaml`

打开您 MacBook 本地的配置文件 `~/.dsh/settings.yaml`，写入 `scnet-llm` 模型提供商配置：

```yaml
ui-onboarding:
  welcomeNoticeVersion: 2026-08-13.1

llm-pi-ai:
  providers:
    scnet-llm:
      displayName: "SCNet (云端大模型)"
      api: "openai-completions"
      apiKeyEnv: "SCNET_API_KEY"
      baseURL: "https://api.scnet.cn/api/llm/v1"
      models:
        - id: "DeepSeek-V4-Pro"   # ⚠️ 注意：大小写必须严格匹配
          name: "DeepSeek V4 Pro (深度推理/长文本)"
          contextWindow: 131072
        - id: "Qwen3-30B"         # ⚠️ 注意：大小写必须严格匹配
          name: "Qwen3 30B (中文写作/学术)"
          contextWindow: 131072
        - id: "SCNet-Max"         # ⚠️ 注意：大小写必须严格匹配
          name: "SCNet Max (平台旗舰/最强Agent)"
          contextWindow: 131072
```

### ⚠️ 踩坑提示：大小写敏感（Case-Sensitive）
在首次连接时，如果配置的模型 ID 为小写（如 `deepseek-v4-pro`），系统会抛出 `422 (Model Not Exist)` 错误。**SCNet 官方 API 接口是严格区分大小写的**。必须填入包含首字母大写的标准 ID（如 `DeepSeek-V4-Pro`、`Qwen3-30B`）。

---

### 步骤二：在 Mac 终端中注入密钥并启动

在超算控制台生成好以 `sk-` 开头的密钥后，在您的 Mac 终端中运行：

```bash
# 1. 将您的密钥写入 Mac 环境变量
echo 'export SCNET_API_KEY="您的真实sk-xxx密钥"' >> ~/.zshrc

# 2. 重新加载配置使之生效
source ~/.zshrc

# 3. 启动 Harness 平台
dsh web
```

打开浏览器访问 `http://localhost:5173`，在左上角的模型选择菜单中，直接切换到 **“SCNet (云端大模型)”**，即可享受超算算力提供的重型大模型推理。

---

## 🎯 科研实战场景展示

通过这个协同网络，科研工作效率实现了翻倍提升：

### 场景一：基金/项目申请书的精细化修改
*   **痛点**：自己写的申请书容易逻辑闭环不强，或学术表达不够精炼。
*   **工作流**：在 Harness 中加载云端 **`DeepSeek-V4-Pro`**。Harness 自动读取您的申请书 Markdown 文件，模型利用极强的长文本逻辑能力，从“立项依据-研究内容-关键科学问题”的多维契合度输出深度的全局修改建议。

### 场景二：学术 PPT 与课件快速大纲提炼
*   **痛点**：要把繁琐的论文内容转化为通俗直观的课件 PPT，手动提炼耗时耗力。
*   **工作流**：使用 **`Qwen3-30B`**。输入您的论文全文，命令 Harness 自动调用格式工具提炼核心逻辑，生成结构化的 Markdown 课件草稿，进一步一键转换为 PPT 文档。

---

## 🏆 结语与展望

这套双层异构 AI 协同网络，完美地将**“本地算力的私密与零延迟”**与**“云端算力的超大规模逻辑深度”**结合在一起。它不仅是 PI 个人的学术效能利器，更避开了繁重的超算服务器运维负担。

未来，当这套自用流程验证成熟后，只需要将接口和密钥分发给感兴趣的学生，即可瞬间完成实验室全体成员的 AI 算力赋能，真正跨入集约化、智能化的 AI 科研新时代。

---

## 参考资料
- SCNet 官方大模型开放平台: [https://api.scnet.cn](https://api.scnet.cn)
- DeepSeek Harness GitHub: [https://github.com/deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness)
