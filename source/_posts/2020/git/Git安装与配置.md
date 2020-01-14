---
layout: post
title: Git安装与配置
categories:
  - Git
tags:
  - Git 安装
  - Git 配置
description: Git 安装与配置中的一些常见场景
date: 2020-01-06 19:14:07
---


## 安装与配置
### Git 安装

Git 的安装非常简单，[Git 的官网](https://git-scm.com/downloads) 详细的介绍了 Git 的安装方式,
当然如果你使用 Git 的场景比较简单，又不想学习 Git 的命令操作方式，也可以安装 Git 的图形界面的客户端，例如 Github-Desktop / source tree / 海龟git 等等

与 Git 命令的操作方式相比，图形界面的操作方式更加直观, 但 Git 命令的操作方式，功能更完善，效率更高.


### Git 配置
Git 有非常多的配置选项，从用户的基本信息，操作习惯，颜色配置都可以高度的可定制化，本节主要讨论一些 Git 中常用的配置选项，更多的配置可以参考官方文档

#### Git 配置基本概念
git 中有系统、全局和本地三个不同级别的配置，Git的配置文件是纯文本的，你可以手动编辑这些文件，但通过 git config 加上对应级别的参数来修改配置文件，是更好的选择。

- 系统配置 --system
配置文件分为 /etc/gitconfig  系统级别的配置, 对全部用户的所有git项目生效
通过 `git config --system ` 进行配置
- 全局配置 --global
配置文件分为  ~/.gitconfig 或者 ~/.config/git/config 对当前用户的所有git项目生效
通过 `git config --global ` 进行配置 
- 本地配置 --local
配置文件为 .git/config ,仅仅对当前的git项目生效,
通过 `git config --local` 或 `git config ` 进行配置

以上三个级别中的每个级别的配置(系统、全局、本地) 都会覆盖上一个级别的配置，所以 .git/config 的配置会覆盖 /etc/gitconfig 的配置

#### Git 常用配置

Git 配置可分为服务端和客户端的，对于普通的 Git 用户来说，只需配置本机的 Git 客户端。虽然 Git 中有丰富的配置选项, 但仅仅只要配置一些常用的选项，就可以打造专属于你的高效 Git 开发环境。

1. 配置用户名称和邮箱
```bash
git config --global user.name xchaoinfo
git config --global user.email xchaoinfo@qq.com
```
2. 配置在Git使用的编辑器
```bash
git config --global core.editor vim
```
完成上面的基本配置，就可以使用Git进行开发工作了，但是更好的提高开发效率，你也可以设置如下的选项

3. 配置提交模板, 让团队的统一提交风格
```bash
git config commit.template ~/.gitmessage.txt
```
4. git log 是否分页，更好的展示 git log 的结果，默认是 less 分页，可以设置为 "" 显示全部内容
```bash
git config core.pager ""
```
5. 自动修改输入错误, 单位为 1/10秒，例如设置5秒后, 自动修正命令后执行
```bash
git config help.autocorrect 50 
```
6. 对常用的操作设置别名，提高 Git 使用效率
首先你可以在 Bash 的配置文件设置 Git 的别名，例如
` alias g="git"`
同时你可以对 Git 的常用命令设置别名, 例如
```bash
git config --global alias.ci commit
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
## 这样可以快速的输入
g ci == git commit 
g co == git checkout
g br == git branch
g st == git status
```

7. 查看配置信息的列表
后面可以加 --global --local --system 查看不同级别的配置
```bash
git config --list [--glolal/local/system]
```


