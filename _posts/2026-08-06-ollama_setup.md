---
layout:     post
title:      llama + Qwen 模型安装备忘
subtitle:   编程总结 | 
date:       2026-08-06
author:     陈陈
header-img:
catalog: true
tags:
    - 编程相关
category: 编程技巧
---
## Ollama + Qwen 模型安装备忘（MacBook Air M5 32G）

> 适用：Apple Silicon 原生 arm64，macOS 12+，Homebrew 已装好  
> 目标：Ollama 跑 qwen3:8b / qwen2.5-coder:14b / qwen3:32b 等，VS Code Continue 调用

---

## 一、安装 Ollama

### 方式 A：Homebrew（推荐）

```bash
brew install ollama
```

### 方式 B：官方安装包

浏览器打开 <https://ollama.com/download> → 选 **Apple Silicon** → 下载 `.dmg` → 拖进 Applications。

### 启动 Ollama 服务

安装完成后，Ollama 会自动注册为后台服务。手动控制：

```bash
# 启动服务（前台，调试用）
ollama serve

# 后台常驻（默认行为，开机自启）
launchctl list | grep ollama   # 确认是否在跑
```

> 服务默认监听 `http://127.0.0.1:11434`，VS Code Continue / Codex CLI 都连这个地址。

---

## 二、拉取模型

### 建议安装的（qwen3:14b）

```bash
# 测试版（简单对话）
ollama pull qwen3:8b-q4_K_M

# 基础版（对话/通用）
ollama pull qwen3:14b-q4_K_M

# 代码专用版（推荐给教学代码、作业评阅）
ollama pull qwen2.5-coder:14b-q4_K_M
```

### 后续可扩展的（按需）

```bash
# 32B 稠密，离线批跑论文评阅（慢，~2.5 tok/s）
ollama pull qwen3:32b-q4_K_M

# 32B 代码版
ollama pull qwen2.5-coder:32b-q4_K_M

# MoE 35B-A3B（激活 3B，速度最快，智能与速度最佳平衡）
ollama pull qwen3.5:35b-a3b-q4_K_M

# DeepSeek 推理风格（看推理链）
ollama pull deepseek-r1:14b-q4_K_M
```

### 查看已装模型

```bash
ollama list
```

### 删除模型（省磁盘）

```bash
ollama rm qwen3:8b-q4_K_M
```

---

## 三、运行与交互

### 命令行直接对话

```bash
ollama run qwen3:14b-q4_K_M
```

进入交互后直接打字，输入 `/bye` 退出。

### 单条提问（非交互）

```bash
ollama run qwen3:14b-q4_K_M "用 Python 写一个 PID 控制器类，带抗积分饱和"
```

### REST API（给 VS Code / Codex CLI 调用）

```bash
# 测试接口是否通
curl http://127.0.0.1:11434/v1/models

# 发一条消息
curl http://127.0.0.1:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen3:14b-q4_K_M",
    "messages": [{"role":"user","content":"你好"}]
  }'
```

---

## 四、VS Code Continue 插件配置

1. 打开 VS Code → 扩展 → 搜 **Continue** → 安装。
2. 左下角 Continue 图标 → 齿轮 ⚙️ → 编辑 `config.json`：

```json
{
  "models": [
    {
      "title": "Qwen3-14B (Ollama 本地)",
      "provider": "ollama",
      "model": "qwen3:14b-q4_K_M",
      "apiBase": "http://localhost:11434/v1"
    },
    {
      "title": "Qwen2.5-Coder-14B (Ollama 本地)",
      "provider": "ollama",
      "model": "qwen2.5-coder:14b-q4_K_M",
      "apiBase": "http://localhost:11434/v1"
    },
    {
      "title": "Qwen3-8B (Ollama 轻量)",
      "provider": "ollama",
      "model": "qwen3:8b-q4_K_M",
      "apiBase": "http://localhost:11434/v1"
    }
  ]
}
```

3. 在 Continue 面板下拉框切换模型即可。

---

## 五、性能调优（M5 Air 无风扇必看）

### 开启闪存注意力（省内存）

```bash
launchctl setenv OLLAMA_FLASH_ATTENTION 1
launchctl setenv OLLAMA_KV_CACHE_TYPE q8_0
```

设置后**重启 Ollama 服务**生效：

```bash
launchctl kickstart -k gui/$(id -u)/com.ollama.ollama 2>/dev/null || pkill ollama
```

### 控制上下文长度（避免 swap）

| 模型 | 建议 num_ctx | 说明 |
|---|---|---|
| 8B | 8192 | 流畅 |
| 14B | 8192 | 上限，再长内存吃紧 |
| 32B 稠密 | **4096** | 超过必 swap 卡死 |
| 35B MoE | 8192-16384 | 激活参数小，能吃长 |

运行时指定：

```bash
ollama run qwen3:32b-q4_K_M --num_ctx 4096
```

或在 Modelfile 里写死（见第六节）。

---

## 六、用 Modelfile 固化评阅规则（Skill 化）

### 论文评阅模型

新建文件 `~/lab-ai-skills/review-paper/Modelfile`：

```dockerfile
FROM qwen3:32b-q4_K_M
PARAMETER num_ctx 4096
PARAMETER temperature 0.3
SYSTEM """你是我实验室的论文评审助手。按以下规则评审：
1. 先给推荐意见（接收/小修/大修/拒稿）
2. 列 3-5 条主要贡献点
3. 列扣分点，每条注明对应评分标准条款
4. 对方法新颖性、实验充分性、与飞行器控制领域相关性分别打分(1-5)
5. 输出严格 Markdown，不寒暄"""
```

构建并运行：

```bash
ollama create lab-review-paper -f ~/lab-ai-skills/review-paper/Modelfile
ollama run lab-review-paper
```

### 周报评阅模型（同理）

`~/lab-ai-skills/review-weekly/Modelfile`：

```dockerfile
FROM qwen3:14b-q4_K_M
PARAMETER num_ctx 8192
PARAMETER temperature 0.3
SYSTEM """你是我实验室的周报评阅助手。检查每份周报是否包含：
1. 本周完成的具体工作（带量化数据）
2. 遇到的阻塞与求助项
3. 下周计划（可验收的里程碑）
对缺失项扣分并给出修改建议。输出 Markdown。"""
```

```bash
ollama create lab-review-weekly -f ~/lab-ai-skills/review-weekly/Modelfile
```

---

## 七、M5 Air 32G 实测社区基准（Q4_K_M，tg128）

| 模型 | 内存占用 | 生成速度 | 定位 |
|---|---|---|---|
| Qwen3-7B / Qwen2.5-Coder-7B | ~4.5G | 30-50 tok/s | 日常问答、改小函数，替代网页版元宝手感 |
| **Qwen3-14B / Qwen2.5-Coder-14B** | ~9-10G | 13-20 tok/s | **最推荐常驻档**，代码+评阅周报够用 |
| DeepSeek-R1-Distill-14B | ~9G | 12-18 tok/s | 看推理链风格 |
| **Qwen3-32B 稠密 Q4** | ~18.6G | **2.5 tok/s** | 慢速批处理，跑论文评阅 SKILL 离线批跑 |
| Qwen2.5-Coder-32B Q4 | ~18.7G | 2.5 tok/s | 同上加代码语境 |
| **Qwen3.5-35B-A3B MoE（激活3B）Q4** | ~20.7G | **31 tok/s** | **32G 上智能与速度的最佳平衡点** |
| Gemma-4-26B-A4B MoE | ~16G | 16 tok/s | 长文写作备选 |
| 70B 级（任何量化） | ≥35G | 装不下，swap 到个位 tok/s | **别试** |

### 关键结论

- **日常交互锁定 14B**：速度够快、质量够用、内存留余量给 macOS。
- **追求质量短时批跑用 32B 稠密**：晚上插电合盖挂机跑论文评阅，不交互等结果。
- **想要「32B 级智能 + 能聊天」上 MoE 35B-A3B**：激活参数只有 3B，绕开内存带宽墙，31 tok/s 流畅。
- **70B 死心**：32G 统一内存装不下，强灌 Q2 走 swap 到个位 tok/s，毫无意义。

---

## 八、M5 Air 无风扇保命三原则

1. **上下文收紧**：32B 跑评阅时 `num_ctx` 设 4096，14B 可到 8192，MoE 可 8192-16384。开太长 → 内存压力 → swap → 速度腰斩。
2. **避免连续高负载 >10 分钟**：稠密 32B 批跑论文评阅时，垫散热支架、合盖用外接屏、跑完立刻 `ollama stop` 释放。M5 Air 8-15 分钟持续推理必触热降频，降幅 30-50%。
3. **常驻只留一个模型**：别同时 load 14B+32B，统一内存吃紧会互踢。切换就 `ollama run` 按需拉起。

---

## 九、常用命令速查

```bash
ollama list                    # 查看已装模型
ollama pull <模型名>           # 下载模型
ollama run <模型名>            # 交互对话
ollama run <模型名> "问题"     # 单条提问
ollama rm <模型名>             # 删除模型
ollama stop <模型名>           # 卸载模型释放内存
ollama create <新名> -f <Modelfile>  # 用 Modelfile 构建自定义模型
ollama show <模型名>           # 查看模型信息
curl 127.0.0.1:11434/v1/models # 检查 API 是否在线
```

---

## 十、故障排查

| 现象 | 原因 | 解决 |
|---|---|---|
| `command not found: ollama` | PATH 未刷新 | `source ~/.zshrc` 或重开终端 |
| Continue 连不上 | Ollama 服务没起 | `launchctl list \| grep ollama` 确认，或 `ollama serve` |
| 32B 跑着跑着卡死 | 内存 swap 爆了 | `ollama stop` 全部，降 `num_ctx` 到 2048 重试 |
| 模型下载中断 | 网络抖动 | 重新 `ollama pull`，支持断点续传 |
| M5 烫到降频 | 无风扇持续高负载 | 垫散热支架、合盖外接屏、缩短单次任务到 10 分钟内 |

---

*最后更新：2026-08-06 · MacBook Air M5 32G · Ollama arm64 原生*
