---
title: "Github 分支操作"
date: 2017-07-27 14:56:16
abbrlink: 1371
draft: false
categories: ["学习笔记"]
tags: ["git"]
---

## 查看分支
### 查看本地分支
使用 git branch命令，如下：
```bash
    $ git branch
    * master
```
\*标识的是你当前所在的分支。
### 查看远程分支
命令如下：
```bash
    git branch -r
```
### 查看所有分支
命令如下：    
```bash
    git branch -a
```

<!-- more -->

## 本地创建新的分支
命令如下：
```bash
    git branch [branch name]
```
例如：
```bash
    git branch gh-dev
```

## 切换到新的分支
命令如下：
```bash
    git checkout [branch name]
```
例如：
```bash
    Ricky@DESKTOP-1QPASTR MINGW64 /f/Git_Studio/design-patterns (master)
    $ git checkout gh-dev
    Switched to branch 'gh-dev'    
    Ricky@DESKTOP-1QPASTR MINGW64 /f/Git_Studio/design-patterns (gh-dev)
```

## 创建+切换分支
创建分支的同时切换到该分支上，命令如下：    
```bash
    git checkout -b [branch name]
```
git checkout -b [branch name] 的效果相当于以下两步操作：
```bash
    git branch [branch name]
    git checkout [branch name]
```

## 将新分支推送到github
命令如下：
```bash
    git push origin [branch name]
```
例如：
```bash
    git push origin gh-dev
```

## 删除本地分支
命令如下：
```bash
    git branch -d [branch name]
```
例如：
```bash
    git branch -d gh-dev
```

## 删除github远程分支
命令如下：
```bash
    git push origin :[branch name]
```
分支名前的冒号代表删除。   
例如：
```bash
    git push origin :gh-dev
```
>注意：通过 Pull request 创建的新分支默认会有一个 Pull request 的 Open 状态标识，这种状态下的分支是无法删除的（页面操作与命令行操作都无法删除），需要到 Pull request 选项里面将 Open 改为 close 后才可删除。

## 下载github某个单独分支
```bash
    git clone -b gh-pages https://github.com/kuleyu/hexolog.git
```
> 其实没有这个说法，这个命令同样也会把所有分支下载下来，只不过下载后的当前分支是这个指定的分支而已。