---
layout:     post
title:      Atom + Latex 编译环境设置
subtitle:   编程总结 | 
date:       2020-11-20
author:     陈陈
header-img: 
catalog: true
tags:
    - 编程
    - Latex
    - Atom

---

## 一、Atom 介绍 
Atom是一款基于GitHub开发的开源跨平台代码编辑器。它具有内置的软件包管理器、嵌入式Git控件、语法突出显示和分屏等常用功能。

网址：https://atom.io/

主要优点：  
1）跨平台支持（Windows、Mac、Linux）；   
2）界面简洁美观；   
3）代码开源、免费使用；  
4）支持各种编程语言的代码高亮和代码补全；   
5）支持Latex编译、预览；   
6）原生Git的支持；  
7）原生Markdown支持（本篇日志就是在Atom上写的）；   
8）基于GitHub社区开发，插件丰富。

## 二、设置 Atom + Latex 编译环境

### 2.1 安装texlive 
Ubuntu 中安装texlive，只需要一句命令：   
sudo apt-get install texlive-full

### 2.2 安装 Atom 
详见Atom官网：  
https://flight-manual.atom.io/getting-started/sections/installing-atom/

具体步骤：   
Step 1 : Add repository from official Atom Site   
wget -qO -https://packagecloud.io/AtomEditor/atom/gpgkey | sudo apt-key add -     
sudo sh -c 'echo "deb [arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main" >
/etc/apt/sources.list.d/atom.list'

Step 2 : Update the Ubuntu System   
sudo apt-get update  

Step 3 : Install Atom Editor   
sudo apt-get install atom

#### 2.2.1 安装 Latexmk  
sudo tlmgr install latexmk


#### 2.2.2 安装 Latex 插件 
左上方标签：Edit -> Preferences -> Packages. [快捷键是ctrl + comma]

安装几个插件：  
latex   
pdf-view   
language-latex   
autocomplete-latex

[2020-11-29 updated: 另一个编译LaTex的插件是"atom-latex"，不需要再装pdf-view, 且支持pdf预览]

#### 2.2.3 设置 Latex 工具链（可跳过）
点击 "latex" package， Setting -> Engine ->  
选取所需的编译工具。例如换成 xelatex。

#### 2.2.4 设置拼写检查
默认的拼写检查不包括.tex文件。首先在terminal 给Ubuntu系统安装字典：

sudo apt-get install hunspell-en-gb  
sudo apt-get install myspell-en-gb

打开“spell-check” package(默认安装)。  
Settings -> Grammars,  
将 "text.tex.latex" 加入语法的list。  
其他格式文件若希望拼写检查，也需要加入这个list，例如“text.md”。

### 2.3 编译 Latex 文件
使用Atom打开待编译的.tex文件。以上安装完成以后，Atom左上方主标签Packages中会出现Latex标签。点击Latex -> Build 就可以了。
