---
layout:     post
title:      Atom + Markdown 配置
subtitle:   学习笔记
date:       2020-11-25
author:     陈陈
header-img:
catalog: true
tags:
    - 编程相关

---
Markdown是一种纯文本格式的标记语言。它通过简单的标记语法，使得普通文本内容具有一定的格式。
类似LaTex，但Markdown一般以HTML格式发布，主要用以生成网页，如网络日志。

优点：  
1. 极简主义。纯文本编写，语法简洁。
2. 兼容HTML，可直接在网络上发布，生成编辑后效果。
3. 可跨平台使用。
4. 越来越多的网站支持Markdown (GitHub, 简书, Moodle, Reddit等等.

Markdown 语法可以Google一下，其中希腊字符和数学表达式与LaTex语法基本一样。在Atom上使用Markdown需以下配置:
## Install the packages:
* language-markdown
* markdown-writer
* markdown-preview
* markdown-preview-enhanced

## Preview your Markdown

1. Open or create a Markdown file (.md extension)
2. Use the menu Packages > Markdown Preview Enhanced
3. Click Toggle

## Spell-check
1. Open “spell-check” package
2. Click Settings $\to$ Grammars,
3. Add text.md into the list.

## LaTeX style math
To enable LaTeX rendering by default search for **Markdown Preview Plus** in the Filter packages input dialogue of the **File › Settings** menu and tick **Enable Latex Rendering By Default**.
