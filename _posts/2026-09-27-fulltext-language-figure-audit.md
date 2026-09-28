---
layout:     post
title:      论文全文排查实录与常见问题
subtitle:   学术写作 | 翻译腔、代号与图表陷阱
date:       2026-09-27
author:     陈陈
header-img: img/post-bg-coffee.jpeg
catalog: true
category: 科研记录
tags:
    - 科研笔记
    - 学术写作
    - 扑翼气动
---

编者按：本文是[《从学生初稿到修改稿：论文摘要修改实录与经验总结》]({{ site.baseurl }}/2026/08/19/abstract-revision/)的续篇。上一篇解决"写什么"——把摘要从参数流水账重构为贡献导向；这一篇解决"怎么写"。当我们拿到完整的 LaTeX 初稿和编译后的 PDF 时，发现全文仍存在大量翻译腔、术语不透明、图表标题不规范、内部代号混乱。这些不解决，审稿人很可能以"语言质量差""可读性低"为由要求大修甚至拒稿。文中英文例句均为原文照录，用作教学对照。面向课题组学生。

## 一、问题：摘要只是冰山一角

摘要改好了，不代表全文能读。真正的排查要从第一句做到最后一张图。下面按"隐蔽程度"和"对审稿的影响"分类，给出典型错误与改法。

## 二、翻译腔与搭配：最隐蔽的拒稿理由

这类错误语法没错、意思也对，但母语者不会那样写，读起来就是"不像人话"。

**案例 1｜引言首句。** 原文：

> Flapping wings generate lift and thrust through **strongly unsteady**, three-dimensional flows **whose loads** depend jointly on…

两处问题。`strongly unsteady` 是"强烈的非定常"的中式直译，英文流体力学文献更常用 `highly unsteady`，或直接用 `unsteady`——非定常本身已隐含时变。`whose loads` 语法上指代 flows，但载荷作用在机翼上、不是流体上，逻辑错位。改为：

> Flapping wings generate lift and thrust through **highly unsteady**, three-dimensional flows. **The resulting aerodynamic loads** depend jointly on…

**案例 2｜搭配不当。** `CFD has consequently become an important route for resolving…` 中的 `route` 指物理路径，此处应表"方法/工具"，改 `tool`、`approach` 或 `means`。

**案例 3｜口语化。** `…the selected model form and validation domain matter` 中的 `matter` 在学术写作中偏口语，改 `play a critical role` 或 `significantly affect the results`。

**案例 4｜用词误导。** `The opposite velocity trends follow from normalization rather than contradictory data` 会让人以为数据本身冲突，其实只是归一化导致趋势相反。改 `contradictory trends` 或 `apparently contradictory observations`。

**案例 5｜拟人化。** `The other one-factor sequences also resist simple generalization` 中的 `resist` 拟人，虽可理解，但更客观的说法是 `do not lend themselves to simple generalization`。

> **翻译腔最隐蔽：语法没错、意思也对，但母语者不会那样写。**

## 三、代号与术语：读者最大的困惑

**案例 6｜内部工况代号满天飞。** 全文出现 `case A7`、`A4`、`E15`、`E16`、`F9`、`G1`、`G2` 等 72 个工况代号，却从未解释命名规则。读者既对不上参数，也不知道 A/B/C/D 各代表什么——实际是单因素变化序列：A 速度、B 频率、C 振幅、D 攻角。改法有三：在 Results 3.1 节开头明确定义四个系列；补一张附录表，列出全部 72 个工况的代号、完整参数、所属系列与数据划分角色；首次提及某代号时用括号给出关键参数，例如 `F9 (U∞ = 12 m/s, f = 6 Hz, θm = 45°, α = 10°)`。

**案例 7｜软件术语未解释。**

- `native samples`（2.3 节）指 CFD 原始时间步输出，读者未必理解，改 `original CFD time-step samples` 或首次出现时定义。
- `refinement-transition length was 3`（2.2 节）是 XFlow 网格自适应参数，加括号说明它控制网格由粗到细过渡的单元数。
- `frozen` SR expression（摘要、2.4 节）是符号回归术语，指选定表达式结构后固定，补 `i.e., its mathematical form was fixed`。
- `development set`（2.4 节）由 49 training + 8 validation 组成，首次出现应写成 `the 57 development conditions (49 training + 8 validation)`。

> **读者读不懂代号，不是读者的问题，是作者把工程台账当成了论文。**

## 四、图表标题与视觉编码：能不能脱离正文读懂

**案例 8｜嵌套括号错误。** 图 4、图 5 标题用了 `(\textbf{(a)})` 语法，PDF 输出成 `((a))`，不符合学术规范。正确写法：

```text
(a) Cycle-averaged coefficients for the A-series velocity sequence; (b) Mean forces for the A-series velocity sequence.
```

**案例 9｜视觉编码过载。** 图 1c 同时用颜色表示 α、符号大小表示 θm、形状表示角色，三重编码在静态图中极难解读。建议简化（如只用形状区分角色），并配附录表，确保不看正文也能大致读懂。

**案例 10｜参数来源缺失。** 轴向参考面积 $S_x = 0.012\,\text{m}^2$ 突然出现、未交代来历；推力系数用 frontal area、升力系数用 planform area，读者会疑惑为何不统一。建议加注 `following XFlow's default frontal-area normalization`，点明这是软件约定而非作者随意选择。

> **图要能脱离正文独立读懂——标题、编码、来源缺一不可。**

## 五、拼写与长句：低成本、高印象分

**案例 11｜拼写错误。** Discussion 中 `in a directly evaluatle equation` 应为 `evaluable`。

**案例 12｜长句堆砌。** Introduction 中一句长达 6 行，含 4 个并列动词（distinguish、separate、compare、report），阅读负担极重。拆成两句，或改为编号列表。

## 六、排查清单：按优先级逐项过

| 问题类型 | 优先级 | 关键动作 |
| --- | --- | --- |
| 代号系统混乱 | 最高 | 补附录工况表，定义系列命名规则 |
| 翻译腔 / 搭配不当 | 高 | 逐句朗读，替换非地道搭配，拆分长句 |
| 术语未定义 | 高 | 首次出现时加简短解释 |
| 图表标题与视觉编码 | 中 | 修正嵌套括号，简化编码 |
| 拼写 / 语法 | 中 | 拼写检查工具 + 人工校对 |

> **判断是不是"人话"，最快的办法是朗读，再到 Google Scholar 搜这个搭配有没有出现在已发表文献里。**

## 七、给学生的三条建议

1. **写完摘要，更要通读全文。** 每一句话、每一个符号、每一幅图，都站在"第一次读这篇论文的陌生人"的角度去审视。如果连自己都要翻回前文才想起某个代号，读者更会如此。
2. **从项目开始就维护"术语表"和"代号表"。** 记录每个内部代号（工况 ID、网格设置名）对应的真实参数，完稿前转成附录表。这是提升可读性最省力的办法。
3. **警惕"机器翻译感"。** 即便用英文直接写作，思维习惯也会带出中式搭配。自查三招：把搭配丢进 Google Scholar 看已发表文献里有没有；朗读，拗口就重写；请母语者或有经验的师兄师姐润色。

## 八、结语：从"写什么"到"怎么写"

上一篇解决"写什么"——结构重组；这一篇解决"怎么写"——语言与图表规范。下次投稿前，拿这份清单逐项过一遍自己的论文。

> **学术写作不是把中文翻成英文，而是用国际同行能轻松读懂的方式，把发现讲清楚。**

*下一篇预告：如何设计附录表格与图表，让论文的"可重复性"经得起审稿人推敲。组会可继续讨论。*
