---
layout: post
title: 'Travis-CI 自动部署 Hexo 静态博客到 Github Pages'
categories:
  - 开发部署
  - Travis-CI
tags:
  - Travis-CI
  - Hexo
  - Github-Pages
description: 'Travis-CI 自动部署 Hexo 静态博客到 Github Pages, 并且轻松的实现源代码管理'
date: 2020-01-01 20:21:12
---

最近花了不少时间, 重新整理了几年前的基于 Hexo 构建的静态博客, 更新了 Hexo 的版本, 主题切换为配置更为方便的 Next, 并且通过 Travis-CI 来自动部署 Github Pages, 完全基于 Git 对博客的源码进行管理, 每次写好博客, 只需要 `git push`把博客的源码推送到 Github的远端仓库, Travis-CI 会自动完成部署.

在部署过程中发现好多网上的教程比较老了, 并且官方的文档也有些写的不够详细, 踩了一些坑, 特此记录下。

## 目录
- [Github Pages 的类型](#Github-Pages-的类型)
- [Travis-CI 简介](#Travis-CI-简介)
- [Travis-CI 部署 Github Pages](#Travis-CI-部署-Github-Pages)

## Github Pages 的类型

从 [Github Pages](https://pages.github.com/) 的官方主页可知, Github分为用户(User)和项目(Project)两个大的类型,

- 用户 Github Pages 用于介绍个人或机构的相关的信息, 地址为 `https://<username>.github.io`
- 项目 Github Pages 用于介绍项目的情况, 地址为 `https://<username>.github.io/<project-name>`

### 用户 Github Pages
用户的 Github Pages 需要建立一个名称为 `<username>.github.io`的 Github repo, 并且 Github Pages 必须部署在 `<username>.github.io` reop 的 master 分支上.

![ ](/images/2020/github-pages-master.jpg)

### 项目 Github Pages
你可以为你的每个 Github repo 部署 Github Pages 用于介绍项目的具体情况, 不同于用户 Github Pages 必须部署在 master 分支, 项目的 Github Pages 的可以部署在如下位置
- master 分支
- master 分支 master/docs 文件下
- gh-pages 分支

![ ](/images/2020/github-pages-proj.jpg)

## Travis-CI 简介

Travis CI 是持续集成服务, 可以绑定 Github 项目,  简单来说就是你每次提交新的代码后，Travis CI 自动化完成构建测试等一系列的操作, 这样每次小的更新都经历完整的测试, 可以尽早的发现 Bug, 提高软件开发的效率.

## Travis-CI 部署 Github Pages

### 1. 创建一个 Github repo 并且同步代码到 github 上
1. 我们要部署 Hexo 生成的静态博客, 建议新建一个名称为 `<username>.github.io` 的 Github repo, 这里的 `<username>` 是你的 Github 用户名称, 例如我的Github用户名是 xchaoinfo , 我建立的 repo 是 `xchaoinfo.github.io`.
2. 建立一个分支用来管理博客的配置和 Markdown 文件, 例如，xchaoinfo 建立了一个名称 blog-source 的分支, 这样方便管理博客的源代码.
3. 将你的 Hexo 站点文件夹推送到 blog-source 分支中。默认情况下不应该 public 目录将不会被推送到 repository 中，你应该检查 .gitignore 文件中是否包含 public 一行，如果没有请加上。

### 2. 设置 Travis CI 服务

1. 将 [Travis CI](https://github.com/marketplace/travis-ci) 添加到你的 GitHub 账户中。
2. 前往 GitHub 的 [Applications settings](https://github.com/settings/installations)，配置 Travis CI 权限，使其能够访问你的 repository。
3. 你应该会被重定向到 Travis CI 的页面。如果没有，请 [手动前往](https://travis-ci.com/)。
4. 在浏览器新建一个标签页，前往 GitHub [新建 Personal Access Token] (https://github.com/settings/tokens)，只勾选 `repo` 的权限并生成一个新的 Token。Token 生成后请复制并保存好。
5. 回到 Travis CI，前往你的 repository 的设置页面，在 Environment Variables 下新建一个环境变量，Name 为 `GH_TOKEN，Value` 为刚才你在 GitHub 生成的 Token。确保 `DISPLAY VALUE IN BUILD LOG` 保持 不被勾选 避免你的 Token 泄漏。点击 Add 保存。

### 3. 编辑 Travis CI 部署文件
在你的 Hexo 站点文件夹中新建一个 `.travis.yml` 文件：
```yml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - blog-source # 仅从 blog-source 分支构建
script:
  - hexo generate # 通过 hexo generate 生成静态文件 static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  committer_from_gh: true
  target_branch: master  # 部署到 master 分支
  on:
    branch: blog-source
  local-dir: public
```
将 .travis.yml 推送到 blog-source 分支中。

Travis CI 应该会自动开始运行，并将生成的文件推送到同一 repository 下的 master 分支下，前往 `https://<username>.github.io` 查看你的站点是否可以访问。这可能需要一些时间。