---
layout:     post
title:      变幅扑翼摘要修改实录
subtitle:   学术写作 | 从实验流水账到贡献导向
date:       2026-09-28
author:     陈陈
header-img: img/post-bg-coffee.jpeg
catalog: true
category: 科研记录
tags:
    - 科研笔记
    - 学术写作
    - 扑翼气动
---

编者按：本文记录一次会议论文摘要的修改过程，对象是课题组学生投往某国际会议的一篇扑翼飞行器论文——*A Variable-amplitude Flight Strategy Study for Flapping Wing Aircraft Vehicle*。

## 一、摘要原稿：

下面是学生的原始初稿：

> This study presents Aquila-S, a single-drivetrain flapping-wing platform for regulating aerodynamic output and comparing electrical input power under equivalent mean-force constraints. A Pixhawk–STM32G4 architecture supports continuous full-amplitude flapping, low-frequency variable-amplitude flapping, phase-locked gliding, and periodic flap-gliding. Source-code co-simulation evaluated mode scheduling and phase control, while fixed-bench tests assessed aerodynamic forces and electrical input power. Variable-amplitude flapping provided adjustable low-output operation, but repeated drivetrain reversal limited the attainable frequency. Static screening identified θ = −10.0° as the target glide phase for subsequent flap-glide tests. These tests used 3 m/s inflow, a 30° body installation angle, and 3 Hz active flapping. At a 30% glide fraction, cycle periods of 4 and 6 s satisfied the prescribed mean-force tolerances. Relative to continuous flapping, these conditions reduced mean electrical input power by 20.1% and 19.8%, respectively. Force-matched flap-glide operation offers a promising approach to reducing mean electrical input power.

逐句诊断：

| 句 | 问题 |
| --- | --- |
| 1 | `single-drivetrain` 不是领域标准术语；`Aquila-S` 项目代号在摘要中不传递信息；核心结论被埋到最后一句 |
| 2 | 四种模式列举尚可，但缺乏与后文的衔接线索 |
| 3 | `Source-code co-simulation` 含义模糊，实际是软件程序在环仿真 |
| 4 | 传动反转限制频率不是什么优点，在摘要中突出强调没有必要|
| 5 | `Static screening` 同样含义模糊，读者只能推测其代表的意思 |
| 6–9 | 参数堆砌（3 m/s、30°、3 Hz、4 s、6 s），具体数字 20.1%、19.8% 淹没在细节中 |
| 10 | 结论笼统，`Force-matched` 术语突兀，没有顺畅地收尾 |

## 二、结构重组：

学术摘要的第一句就应该告诉读者：你做了什么，最核心的结论是什么。原稿把"扑-滑结合可降低功率"埋在最后一句，改后将定性结论（`can significantly reduce`）前置到首句。

后续按“平台→控制→验证→调节机制→实验→量化结果→意义”逻辑递进展开。每句话为下一句话搭桥：第二句用 `The prototype` 回指首句；第三句用 `then` 顺承验证步骤；第四句先声明调节能力，为实验做铺垫；第五句定量测试，承接上句。

## 三、做减法：

以下信息被删去：

- **`single-drivetrain`**：非标准术语，不是本文贡献，读者不关心。
- **`Aquila-S` 项目代号**：摘要空间有限，代号不传递信息，正文再提。
- **`Static screening identified θ = −10.0°`**：这个细节不重要且难以理解。摘要不应被旁支分散注意力。
- **具体工况参数**：原稿列了 3 m/s、30°、3 Hz、4 s、6 s。摘要不是实验报告——读者若对条件感兴趣会查正文。最终仅保留"30% 滑翔占比"和"约 20% 节能"两个关键数字。
- **传动反转限制频率**：负面信息，更不需要在摘要体现。

> **删掉"自己觉得重要"但读者并不关心的细节，是学术写作的第一道门槛。**

## 四、修改稿：

> This study develops a variable-amplitude flapping-wing prototype and demonstrates that flap-glide operation can significantly reduce mean electrical input power. The prototype employs a Pixhawk–STM32G4 control architecture that implements four flight modes: continuous full-amplitude flapping, low-frequency variable-amplitude flapping, phase-locked gliding, and periodic flap-gliding. A Software-in-the-loop (SITL) co-simulation platform is then established to validate the mode scheduling and phase control accuracy. Variable-amplitude flapping adjusts aerodynamic output and mean electrical power via amplitude and frequency modulation. Wind-wall-based fixed-bench tests quantify aerodynamic forces and electrical power under equivalent mean-force constraints. For the selected operating conditions, a 30% glide fraction reduces mean electrical input power by approximately 20% relative to continuous flapping. These results establish flap-glide scheduling as a promising strategy for power-efficient flapping-wing flight.

逐句功能：

| 句 | 功能 | 要点 |
| --- | --- | --- |
| 1 | 核心贡献 | `significantly reduce` 定性先行，不堆砌数字 |
| 2 | 平台能力 | `prototype` 回指首句，冒号引出四种模式 |
| 3 | 验证手段 | `then` 顺承，明确验证对象是模式调度与相位控制 |
| 4 | 调节机制 | 声明变幅扑动能力，为实验做铺垫 |
| 5 | 实验方法 | 风墙台架定量测试 |
| 6 | 关键结果 | 仅留两个核心数字（30%、~20%），其余工况不列 |
| 7 | 意义提升 | 收尾点明扑-滑调度是高效飞行策略 |

## 五、一些体会和建议

1. **摘要不是缩小版的全文。** 它是一份自给自足的贡献声明——读者不用读正文也应该能说出你做了什么、得到了什么。把所有细节塞进去不等于"信息丰富"，等于"没有重点"。
2. **做减法的判断力比做加法的勤奋更重要。** 写摘要前的第一件事不是打开正文复制粘贴——而是问自己：如果只能留三个信息点，会是哪三个。非关键信息都删减。
3. **句子之间要有逻辑连接。** 写完一段后，一定要自己多读几遍。如果每句话都彼此独立，只是一股脑地把意思说完了而没有关联和逻辑，就需要重写。多用连接词让句子"流动"起来。

做实验、跑仿真、分析数据不是科研的全部，如何展示自己做的工作同样重要。我们要学会从读者的角度来思考。对于一篇摘要，绝大多数读者并不关心实验细节，而只关心三件事：你做了什么、得到了什么、为什么重要。