---
title: git 基础
date: 2021-03-08 20:42:58
tags:
  - git
---

主要概念：

- 工作区： 平时改写文件的区域，可以理解为存放文件当前状态的区域。
- 暂存区： 可以将工作区的文件添加到暂存区保存文件的改动准备提交到本地分支上。
- 版本库（本地仓库）：版本库存放着多条本地分支，贮藏等。位于 `.git` 文件夹里。
- 远程仓库：区别于本地仓库，位于外部（服务器）的仓库。

## 新建本地仓库（初始化）
首先进入到项目的根目录，再进行初始化
```powershell
$ cd myProject
$ git init
```

## 保存修改到暂存区

我们可以用 `git add`命令将文件添加到暂存区

```powershell
$ git add demo.txt
```

## 提交到版本库

我们使用 `git commit`命令将暂存区提交到版本库

```powershell
$ git commit -m "本次提交的简单说明"
```

## 分支

### 创建分支

首先，我们创建 dev 分支，然后切换到 dev 分支：

```powershell
$ git checkout -b dev
```

git checkout 命令加上-b 参数表示创建并切换，相当于以下两条命令：

```powershell
$ git branch dev
$ git checkout dev
```

### 查看分支

用 git branch 命令查看当前分支：

```powershell
$ git branch
* dev
  master
```

git branch 命令会列出所有分支，当前分支前面会标一个\*号。

### 切换分支

1. checkout
   切换回 master 分支:

```powershell
$ git checkout master
```

2. switch (推荐使用)
   创建并切换到新的 dev 分支

```powershell
$ git switch -c dev
```

直接切换到已有的 master 分支

```powershell
$ git switch master
```

### 合并分支

1. merge

`git merge` 命令用于合并指定分支到当前分支。<br>
直接把 master 指向 dev 的当前提交

```powershell
$ git merge dev
```

2. rebase

`git rebase` 命令用于将**当前分支** 与 **将要变基的分支** 的**共同祖先** 开始提取 当前分支的提交(commit)，再将祖先指向 **将要变基的分支**

> 请只在自己的分支上使用，否则会对团队中的其他人造成麻烦。

例如将当前分支变基到 `master`

```powershell
$ git rebase master
```

### 删除分支

删除 dev 分支:

```powershell
$ git branch -d dev
```

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除

## (status)储藏

- 储藏当前未提交的内容

```powershell
$ git stash
```

- 查看储藏列表

```powershell
$ git stash list
```

- 恢复储藏
  1.apply+drop

```powershell
$ git stash apply
```

但是`apply`不会删除 stash 内容，需要`drop`

```powershell
$ git stash drop
```

2.pop
恢复的同时把 stash 内容也删了

```powershell
$ git stash pop
```

## 复制特定的提交到当前分支

```powershell
$ git cherry-pick <commit>
```
