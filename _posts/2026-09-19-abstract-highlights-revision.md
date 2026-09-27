---
layout:     post
title:      摘要与亮点的修改实录
subtitle:   学术写作 | 从参数流水账到贡献导向
date:       2026-09-19
author:     陈陈
header-img: img/post-bg-coffee.jpeg
catalog: true
category: 科研记录
tags:
    - 科研笔记
    - 学术写作
    - 扑翼气动
---

编者按：本文记录一次摘要与 Highlights 的修改过程，对象是课题组投往 *Theoretical and Applied Mechanics Letters*（TAML）的一篇论文——*Data-driven modeling of cycle-averaged and phase-resolved aerodynamic responses of a three-dimensional rigid flapping wing*。初稿完成后，学生把摘要与 Highlights 交给 AI 助手润色，结果只是替换了几个同义词。真正的问题不在词，而在结构：摘要写成了参数研究的流水账，Highlights 与正文贡献脱节。文中英文摘要与 Highlights 均为原文照录，用作教学对照。面向课题组学生。

## 一、问题：AI 改的是词，没动结构

润色与重写是两回事。同义词替换解决不了"读者读了一半还不知道本文做了什么"的问题。这次要改的不是措辞，而是信息的排布。

## 二、摘要诊断：参数流水账压过了贡献

- **结构失衡。** 四个单因素趋势（速度、频率、幅度、攻角）用分号并列，占据了摘要主体，方法创新被挤到后半段。
- **细节过度。** "between 7.5° and 10°" 这类正文里的离散数据点被放进摘要，既破坏了与其他趋势的详略平衡，又容易被误读成普适规律。
- **翻译腔。** 大量被动语态（was conducted、were used）、中式直译（Along the sampled…、establish complementary data-driven routes）、以及生硬短语（without a monotonic lift benefit）。

初稿摘要：

```text
The aerodynamic response of a flapping wing depends jointly on its operating condition and on the phase within the flapping cycle, posing distinct requirements for interpretable mean-load relations and phase-resolved surrogate models. Here, a data-driven analysis was conducted of a three-dimensional rigid NACA 0014 wing using 72 computational-fluid-dynamics (CFD) conditions spanning freestream velocity, flapping frequency, flapping-angle amplitude, and static angle of attack. Separate reference areas and a propulsion-positive sign convention were used to distinguish dimensional forces from normalized lift and thrust coefficients. Along the sampled one-factor sequences, increasing freestream velocity reduced both mean coefficients while increasing the corresponding forces; increasing frequency strengthened the aerodynamic responses overall; increasing amplitude primarily enhanced thrust without a monotonic lift benefit; and increasing angle of attack raised lift while inducing a thrust-to-drag transition between 7.5° and 10°. For cycle-averaged lift, a frozen symbolic-regression expression achieved a test RMSE of 0.0292 and R²=0.9940, while a second-order polynomial Ridge baseline attained an RMSE of 0.0234. For phase-resolved prediction, a five-network deep-neural-network ensemble achieved range-normalized RMSE values of 1.80% for thrust and 1.77% for lift, compared with 7.66% and 7.07% for a Fourier–Ridge baseline. Errors increased for joint parameter changes and frequency extrapolation. These results establish complementary data-driven routes for explicit mean-load estimation and accurate phase-resolved waveform prediction across the sampled operating conditions.
```

> **摘要不是实验报告，而是贡献宣言。**

## 三、Highlights 诊断：三条亮点没有一条落在核心贡献上

初稿 Highlights：

```text
1. Mean forces and coefficients exhibit distinct responses to freestream velocity.
2. Symbolic regression provides a compact explicit relation for cycle-averaged lift.
3. A DNN predicts phase-resolved loads accurately within explicit validity boundaries.
```

- **亮点 1** 只讲速度对力与系数的不同影响，这是最基础的参数分析，不是本文贡献。
- **亮点 2** 漏掉了与 Ridge 基线的对比，也没给误差量级，信息不足。
- **亮点 3** 的 "within explicit validity boundaries" 不准确——正文并未给出严格边界，只是按测试类别展示了误差递增；且未提集成策略、对比基线与具体精度。

## 四、重写后的摘要与 Highlights

新摘要：

```text
This study presents a data-driven framework for predicting cycle-averaged and phase-resolved aerodynamic responses of a three-dimensional rigid flapping wing. Using 72 computational-fluid-dynamics conditions spanning freestream velocity, flapping frequency, amplitude, and angle of attack, we develop two complementary models: a symbolic-regression expression for mean lift and a five-network deep-neural-network ensemble for phase-dependent thrust and lift. For cycle-averaged lift, the selected explicit expression achieves a test RMSE of 0.0292 and R²=0.9940, while a polynomial Ridge baseline yields a lower RMSE of 0.0234, illustrating a trade-off between interpretability and accuracy. For phase-resolved prediction, the DNN ensemble attains range-normalized RMSE values of 1.80% for thrust and 1.77% for lift, outperforming a Fourier–Ridge baseline (7.66% and 7.07%). Errors are lowest for one-factor interpolation and increase systematically for joint parameter variations and frequency extrapolation, thereby defining empirical validity boundaries. These results establish a reproducible basis for selecting either an explicit mean-load relation or a phase-resolved surrogate in flapping-wing aerodynamic analysis.
```

新 Highlights：

```text
- A data-driven framework couples symbolic regression (explicit) and a DNN ensemble (accurate) for flapping-wing aerodynamics.
- Symbolic regression yields a compact mean-lift expression (RMSE 0.0292), trading interpretability against a more accurate Ridge baseline (RMSE 0.0234).
- The DNN ensemble achieves approximately 2% range-normalized RMSE for phase-resolved thrust and lift, with errors increasing from interpolation to frequency extrapolation.
```

## 五、改法拆解：从"循序渐进"到"倒金字塔"

| 初稿结构（问题） | 新结构（解决） |
| --- | --- |
| 第 1 句：背景铺垫 | 第 1 句：直接点明"本文提出一个数据驱动框架" |
| 第 2 句：数据细节 + 被动语态 | 第 2 句：数据规模 + 两个互补模型（核心贡献） |
| 第 3 句：符号约定（琐碎） | 并入方法描述，不单独成句 |
| 第 4 句：四个趋势流水账 | 删除，压缩为 "spanning freestream velocity…" 一笔带过 |
| 第 5–6 句：SR 与 DNN 结果 | 第 3–4 句：保留关键数字 |
| 第 7 句：误差增加（模糊） | 第 5 句：误差随测试类别变化，明确边界 |
| 第 8 句：泛泛总结 | 第 6 句：落到"选择显式关系或代理模型"的意义 |

> **英文期刊摘要要"倒金字塔"：先亮核心贡献，再给关键证据，最后落到意义。**

去翻译腔只做三个动作：改**主动语态**（We develop、we establish）；换**精准动词**（presents 框架、develop 模型、attains 精度、outperforming 对比、defining 边界）；**删冗余**（Here,、Along the sampled…）。

细节处理遵循一条原则：**摘要里保留精确数字，Highlights 里用约数。** 摘要中的 RMSE 0.0292、1.80%/1.77% 是方法性能的核心证据，必须精确；Highlights 把 DNN 精度改为 "approximately 2%"，符合"亮点"的概括性；7.5°–10° 区间删除，符号回归的 "frozen" 改为更准确的 "selected"。

## 六、给学生的三条建议

1. **摘要先回答一句话。** 如果读者只记住一句，你希望是哪句——把它放在最前面。参数影响、数据细节、符号约定都是支撑材料，能压则压，能删则删。
2. **Highlights 是广告语，不是目录。** 每条要让非专业编辑一眼看懂创新点并愿意点开全文。用具体对比（trading interpretability against accuracy）和关键数字支撑，避免 "explicit validity boundaries" 这类模糊表述。
3. **警惕翻译腔的三种症状。** 过度被动（was conducted / were used）、直译连接词（Along…、Based on… we found that）、抽象名词堆砌（establish complementary data-driven routes）。写完大声朗读，拗口就拆成短句或换主动动词。

> **Highlights 是广告语，不是目录。**

## 七、结语：贡献要替读者总结好

从初稿到终稿，变的不是词，是信息的位置。下次写摘要，先跳出"背景—做法—结果"的惯性，把最硬的成果亮在开头。

> **编辑和审稿人没有义务读完你的摘要去猜你的贡献——你必须替他们总结好。**
