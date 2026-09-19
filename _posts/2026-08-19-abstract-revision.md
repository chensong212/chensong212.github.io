---
layout:     post
title:      从学生初稿到修改稿：论文摘要修改实录与经验总结
subtitle:   科研笔记 | 摘要与亮点修改实录：从“参数流水账”到“贡献导向”
date:       2026-09-19
author:     陈陈
header-img: img/post-bg-coffee.jpeg
catalog: true
category: 科研记录
tags:
    - 科研笔记
    - 学术写作
    - 研究生培养
---

## 1. 背景与原文回顾

近期我们近期拟投稿的一篇英文论文，题为 *“Data-driven modeling of cycle-averaged and phase-resolved aerodynamic responses of a three-dimensional rigid flapping wing”*。初稿完成后，我让学生将论文交给 AI 助手做语言润色，结果 AI 只是机械地替换了几个同义词，完全没有触及根本问题——**摘要写成了参数研究的流水账，Highlights 也与正文贡献脱节**。

### 1.1 初稿摘要（原文）

> The aerodynamic response of a flapping wing depends jointly on its operating condition and on the phase within the flapping cycle, posing distinct requirements for interpretable mean-load relations and phase-resolved surrogate models. Here, a data-driven analysis was conducted of a three-dimensional rigid NACA 0014 wing using 72 computational-fluid-dynamics (CFD) conditions spanning freestream velocity, flapping frequency, flapping-angle amplitude, and static angle of attack. Separate reference areas and a propulsion-positive sign convention were used to distinguish dimensional forces from normalized lift and thrust coefficients. Along the sampled one-factor sequences, increasing freestream velocity reduced both mean coefficients while increasing the corresponding forces; increasing frequency strengthened the aerodynamic responses overall; increasing amplitude primarily enhanced thrust without a monotonic lift benefit; and increasing angle of attack raised lift while inducing a thrust-to-drag transition between 7.5° and 10°. For cycle-averaged lift, a frozen symbolic-regression expression achieved a test RMSE of 0.0292 and $R^2=0.9940$, while a second-order polynomial Ridge baseline attained an RMSE of 0.0234. For phase-resolved prediction, a five-network deep-neural-network ensemble achieved range-normalized RMSE values of 1.80% for thrust and 1.77% for lift, compared with 7.66% and 7.07% for a Fourier–Ridge baseline. Errors increased for joint parameter changes and frequency extrapolation. These results establish complementary data-driven routes for explicit mean-load estimation and accurate phase-resolved waveform prediction across the sampled operating conditions.

### 1.2 初稿 Highlights（原文）

1. Mean forces and coefficients exhibit distinct responses to freestream velocity.
2. Symbolic regression provides a compact explicit relation for cycle-averaged lift.
3. A DNN predicts phase-resolved loads accurately within explicit validity boundaries.

---

## 2. 核心问题诊断

### 2.1 摘要：参数趋势喧宾夺主

- **结构失衡**：全文 8 句话中，第 4 句用分号并列了四个单因素趋势（速度、频率、幅度、攻角），占全文近 40% 篇幅。读者读完一半还不知道文章到底做了什么方法创新。
- **细节过度**：攻角趋势中给出了精确角度范围 “between 7.5° and 10°”，这是正文结果部分的离散数据点，放在摘要中既破坏与其他趋势的详略平衡，又让读者误以为这是普适规律。
- **语言翻译腔**：大量被动语态（“was conducted”, “were used”）、中式直译（“Along the sampled...”, “establish complementary data-driven routes”）、以及生硬短语（“without a monotonic lift benefit”）。

### 2.2 Highlights：与摘要和正文均不匹配

- **亮点 1** 只提了速度对力/系数的不同影响，这是最基础的参数分析，并非文章核心贡献。
- **亮点 2** 忽略了与 Ridge 基线的对比，未提及 SR 表达式的测试误差（RMSE 0.0292），信息量不足。
- **亮点 3** 中 “within explicit validity boundaries” 表述不准确——文中并未给出严格边界，而是通过测试集分类展示了误差递增趋势；且未提及 DNN 的集成策略、对比基线及具体精度。

---

## 3. 修改方案：全新摘要与 Highlights

### 3.1 新摘要（修改版）

> This study presents a data-driven framework for predicting cycle-averaged and phase-resolved aerodynamic responses of a three-dimensional rigid flapping wing. Using 72 computational-fluid-dynamics conditions spanning freestream velocity, flapping frequency, amplitude, and angle of attack, we develop **two complementary models**: a symbolic-regression expression for mean lift and a five-network deep-neural-network ensemble for phase-dependent thrust and lift. For cycle-averaged lift, the selected explicit expression achieves a test RMSE of 0.0292 and $R^2=0.9940$, while a polynomial Ridge baseline yields a lower RMSE of 0.0234, illustrating a trade-off between interpretability and accuracy. For phase-resolved prediction, the DNN ensemble attains range-normalized RMSE values of 1.80% for thrust and 1.77% for lift, outperforming a Fourier–Ridge baseline (7.66% and 7.07%). Errors are lowest for one-factor interpolation and increase systematically for joint parameter variations and frequency extrapolation, thereby defining empirical validity boundaries. These results establish a reproducible basis for selecting either an explicit mean-load relation or a phase-resolved surrogate in flapping-wing aerodynamic analysis.

### 3.2 新 Highlights（修改版）

- **A data-driven framework couples symbolic regression (explicit) and a DNN ensemble (accurate) for flapping-wing aerodynamics.**
- **Symbolic regression yields a compact mean-lift expression (RMSE 0.0292), trading interpretability against a more accurate Ridge baseline (RMSE 0.0234).**
- **The DNN ensemble achieves approximately 2% range-normalized RMSE for phase-resolved thrust and lift, with errors increasing from interpolation to frequency extrapolation.**

---

## 4. 修改思路详解：从“流水账”到“贡献导向”

### 4.1 结构重组（核心转变）

| 初稿结构（问题） | 新摘要结构（解决） |
|---|---|
| 第1句：背景（依赖条件和相位） | **第1句：直接点明“本文提出了一个数据驱动框架”** |
| 第2句：数据细节 + 被动语态 | **第2句：数据规模 + 两个互补模型（核心贡献）** |
| 第3句：符号约定（琐碎） | 合并到方法描述中，不单独成句 |
| 第4句：四个趋势流水账（占40%） | **完全删除**，压缩为引言/正文内容 |
| 第5-6句：SR 和 DNN 结果 | **第3-4句：SR 和 DNN 结果（保留关键数字）** |
| 第7句：误差增加（模糊） | **第5句：误差随测试类别的变化（明确边界）** |
| 第8句：总结（routes across conditions） | **第6句：结论（reproducible basis for selecting models）** |

**为什么这样改？**  
英文期刊摘要更倾向“倒金字塔”：先亮核心贡献，再给关键证据，最后落脚到意义。初稿是典型的“循序渐进”中式写法，编辑和审稿人需要读到一半才能抓住重点。新摘要将参数趋势全部压缩为第2句的 “spanning freestream velocity...”，一笔带过，把节省的篇幅让给方法创新和性能对比。

### 4.2 语言去翻译腔

- **主动语态**：全篇使用 `We develop`, `we establish` 等，避免 `was conducted` 等被动。
- **动词精准**：`presents` (框架), `develop` (模型), `attains` (精度), `outperforming` (对比), `defining` (边界)。
- **逻辑连接**：用 `while` (对比 SR 和 Ridge), `thereby` (误差导致边界), `either...or` (选择意义) 等自然衔接。
- **删减冗余**：去掉 `Here,`, `Along the sampled...`, `creating distinct requirements` 等中式直译。

### 4.3 Highlights 的同步调整

Highlights 必须与摘要的“贡献导向”一致，且每条应是一个独立、自包含的卖点。

- **第1条**：删除旧亮点1（速度趋势），改为突出**框架双模型结构**，点明“显式 vs 准确”的互补性。
- **第2条**：保留 SR 的 RMSE 0.0292，但**补入 Ridge 的 0.0234 作为对比**，明确“可解释性-准确性权衡”。
- **第3条**：将具体数字 1.77–1.80% 改为 **approximately 2%**（只告诉读者大致误差量级），并将误差递增规律（插值→外推）作为边界依据，完全剔除不准确的“explicit validity boundaries”说法。

### 4.4 细节处理原则

- **具体数字**：摘要中保留 SR 和 DNN 的精确 RMSE（因为这是方法性能的核心证据），但 Highlights 中 DNN 精度改用约数，符合“亮点”的概括性要求。
- **攻角推力-阻力转变**：完全删除 7.5°–10° 这个具体区间，因为这不是文章贡献，且数字过于具体。正文 Results 部分已有完整数据，摘要无需重复。
- **符号回归的 “frozen”**：改为 `selected`，更准确。

---

## 5. 给学生们的三点写作建议

1. **摘要不是“实验报告”，而是“贡献宣言”**  
   永远先问自己：如果读者只记住一句话，我希望是哪句？把这句话放在摘要最前面。参数影响、数据细节、符号约定都是支撑材料，能压缩就压缩，能删除就删除。

2. **Highlights 是“广告语”，不是“目录”**  
   每条 Highlight 应该让非专业编辑一眼看懂你的创新点，并愿意点开全文。避免模糊表述（如 “explicit validity boundaries”），用具体对比（如 “trading interpretability against accuracy”）和关键数字（但非全部细节）来支撑。

3. **警惕“翻译腔”的三种典型症状**  
   - 过度被动语态（尤其 “was conducted/were used”）  
   - 直译中文连接词（“Along...”, “Based on... we found that”）  
   - 抽象名词堆砌（“establish complementary data-driven routes”）  
   修改方法：写完摘要后，大声朗读，如果觉得拗口，就拆成短句或换主动动词。

---

## 6. 结语

这篇博客记录了从初稿到终稿的完整修改轨迹。希望同学们在下次写摘要时，能跳出“先介绍背景，再讲做了什么，最后说结果”的惯性，尝试用“倒金字塔”结构，把最硬的成果亮在开头。记住：**编辑和审稿人没有义务读完你的摘要去猜你的贡献，你必须替他们总结好。**

---

*如有任何问题，欢迎在组会中讨论。*