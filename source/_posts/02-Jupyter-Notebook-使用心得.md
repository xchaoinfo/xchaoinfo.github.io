---
layout: post
title: 再谈 Jupyter Notebook 的使用
categories:
  - 开发工具
  - Jupyter
tags:
  - Jupyter Notebook
  - Ipython
  - 开发工具
description: 18年的时候写过一篇简短的 Jupyter Notebook 使用心得, 本篇在原来内容的基础上, 重新梳理内容, 并增加更多的细节, 并且分享自己使用过程中的一些问题.
date: 2020-01-09 22:28:57
permalink: jupyter
---


从事数据分析的工作，Jupyter Notebook 一直是我的工作中的趁手工具, 但是使用过程也有些不如意之处，

欣喜之处颇多, 不如意之处亦有, 故此分享一些新的体会
最近在用Python做一些数据分析和科学计算相关的内容，对于Jupyter Notebook 的使用，有些心得，分享给诸位。


多参考其他人的文章，然后参考官方文档，希望可以写的非常详细的一篇教程

### 安装与配置
### 基本概念介绍
### Ipython介绍
### 快捷键
### Jupyter Lab
### 周边工具

数据分析由于需要用到与科学计算相关的包，而且这些包依赖于C/C++ 的扩展，所以比较常用的 conda 的 Python 的发行包
完整版的 anaconda 一般是包含了 Jupyter 的

安装与配置的过程中，需要注意以下几点

1. 使用pip或者conda一种包管理工具来安装Jupyter，由于Jupyter 依赖大量的第三方包，混合pip和conda来管理依赖会导致Jupyter出问题。

2. 用jupyterthemes配置下适合自己风格的主题。


jupyterlab

3. 由于随着版本的变化，Python第三方包的安装方式可能会有所变化，所以，最好方式是，参考官方文档安装。

 

Jupyter Notebook 的使用

首先，Jupyter Notebook 是由一个Cell（单元）构成的，一共有三种类型的Cell。

code，交互式代码运行环境，对Python来说，这里就是ipython

Markdown，这里的Markdown是带有latex公式输入的增强版的Markdown.

raw text, 纯文本，主要是方便处理大段的文字，避免对特殊字符的转义。

 

因此，想要舒适快捷的使用Jupyter Notebook, 你至少应该熟悉Ipython + Markdown + Latex公式。Ipython中能够提高生产效率的工具，主要是魔法（Macros）命令和代码自动补全和提示。Markdown和Latex公式，只需要记住语法规则和常用的特殊字符的含义。

 

其次，需要理解的是Jupyter Notebook 有编辑和命令两种模式，如果使用过Vim,对这种设计一定不会陌生，这对于提供开发效率非常重要。当进入命令模式后，可以对Cell进行增删改等许多操作，具体的可以在命令模式输入H查看快捷键的说明。例如，可以通过快捷键，在命令和编辑两种模式下快速的切换，在三种Cell 中切换。

 

以下是一些常用的快捷键

 

Esc or Ctrl+M 退出编辑模式，进入命令模式

Enter 进入编辑模式

Shift+Enter 运行 Cell 并且进入下一个 Cell

Ctrl+Enter 运行 Cell

P 进入命令搜索模式(支持模糊匹配, sublime text 也有类似的设计)

H 进入帮助，不用担心记不住如此多的快捷键。

Y (code), M(markdown),R(raw) 三种 cell 切换

 

 

以上内容，能够让你快速高效的使用Jupyter Notebook