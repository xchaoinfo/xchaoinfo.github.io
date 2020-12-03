---
layout: post
title: Git Tag
categories:
  - Git
tags:
  - Git-Tag
  - Git-版本发布
description: git tag 命令用来为 Git 仓库历史记录中的某个节点，指定一个永久的书签，一般情况下用于版本发布的相关事宜。
date: 2020-01-09 20:43:33
---

`git tag` 命令用来为 Git 仓库历史记录中的某个节点，指定一个永久的书签，一般情况下用于版本发布的相关事宜。


Git 标签分为轻量级标签和附注标签两种类型

### 列出标签

1. 列出所有的标签
`git tag`
2. 列出本地的特定版本的标签
`git tag -l "v1.2*"`

### 标签管理

1. 添加轻量化标签
`git tag v0.1`
2. 添加附注型标签
`git tag -a v0.1 -m "v0.1 tag to pushlish"`
3. 为某个历史记录添加标签
`git tag -a v0.2 3b7c315 -m "v0.2"`
4. 删除标签
`git tag -d v0.1`

### 共享标签，将本地的标签同步到远端

1. 共享 v0.1 标签到远端仓库
`git push origin v0.1` 
2. 共享本地所有的标签到远端仓库
`git push origin --tags` 
3. 查看远端仓库的有哪些标签
`git ls-remote`
4. 删除标签, 并且同步到远端仓库

```bash
git tag -d v0.1 
git push origin :refs/tags/v0.1
```

### 从 Tag 开始开发

如果某个版本发布之后，出现了 bug, 可以从 tag 新建一个分支让后开发

```bash
git checkout v0.1
git branch v0.1-fix
```

或者可以简写为
`git checkout -b v0.1-fix v0.1`
