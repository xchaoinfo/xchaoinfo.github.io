---
layout: post
title: Git 分支操作
categories:
  - Git
tags:
  - Git
  - Git分支
description: git-branch
date: 2020-01-14 22:39:04
---
## 强大的分支

### git branch 分支的基本概念

### git branch 分支操作

### 分支工作流

1. 查看分支
git branch 查看本地分支列表
git branch -r 查看远程分支列表
git branch -a 查看所有的分支列表，包括本地和远程

git branch -v 列出当前的所有分支以及它们的最后一次提交
git branch -vv 列出本地分支以及最后一次条件和远程仓库的缩写

git branch --merged 查看哪些分支已经合并到当前分支
git branch --no-merged 查看哪些分支尚未合并到当前分支

1. 新建分支
git branch xxx

2. 切换分支
git checkout xxx

3. 创建并且切换分支
`git checkout -b xxx` 的是如下命令的缩写

```bash
git branch xxx
git checkout xxx
```

3. 删除分支

git branch -d xxx 删除已经合并的分支
git branch -D xxx 强制删除分支, 包括尚未合并的分支

### 远程分支

git remote
git remote show
git remote show origin
git ls-remote 

git remote add hello https://github.com/xchaoinfo/hello

git remote -v


git fetch orgin 拉取远程分支到本地 master 分支

git push --set-upstream origin branch_name

--set-upstream 的意义在于本地分支与远程的分支关联
下次可以直接 git push 完成提交

如果远程分支删除，但是本地关联的分支没有删除，可以使用 git remote prune 来删除


## 5. git 与远程服务通信
### git fetch
### git push
## 6. git 高阶操作
1. fork 后更新原来repo的更新内容

git remote -v

`git remote add analaysisTool http://gitlab.com/xchao/analaysisTool.git`

git remote -v
出现目前源的内容

git fetch analaysisTool
git merge analaysisTool\master


git branch -r 查看远程分支
git branch -a 查看所有分支


## 远端仓库
## Github 协作流程


git push --set-upstream origin branch_name

--set-upstream 的意义在于本地分支与远程的分支关联
下次可以直接 git push 完成提交
### 查看当前有哪些分支
git remote -v

git remote add xchaoinfo http://gitlab.com/xchao/analaysisTool.git

git remote -v
出现目前源的内容

git fetch analaysisTool
git merge analaysisTool\master


git branch -r 查看远程分支
git branch -a 查看所有分支


git push xxx origin xxx