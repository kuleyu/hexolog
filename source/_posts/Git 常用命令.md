---
title: Git 常用命令
tags: git
abbrlink: 7065
date: 2017-07-27 23:21:09
categories: 搬运整理
---

[转载 - Ricky - CSDN博客](http://blog.csdn.net/top_code/article/details/51931589 "Permalink to Git常用命令 - Ricky - CSDN博客")
 
## Git常用命令 

### 新建Repository
```bash
    $ git init
    $ git init [project-name]
    $ git clone [url]
```
例如：   
1、创建本地Repository
```bash
    $ mkdir pro-git
    $ cd pro-git
    $ git init
```
2、clone 远程Repository
```bash
    $ git clone git@github.com:FBing/design-patterns.git
```

<!-- more -->

### 初次运行 Git 前的配置

既然已经在系统上安装了 Git，你会想要做几件事来定制你的 Git 环境。 每台计算机上只需要配置一次，程序升级时会保留配置信息。 你可以在任何时候再次通过运行命令来修改它们。

Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：
1. /etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有 –system 选项的 git config 时，它会从此文件读写配置变量。

2. ~/.gitconfig 或 ~/.config/git/config 文件：只针对当前用户。 可以传递 –global 选项让 Git 读写此文件。

3. 当前使用仓库的 Git 目录中的 config 文件（就是 .git/config）：针对该仓库。

每一个级别覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。

#### 用户信息

当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：
```bash
    $ git config [--global] user.name "Ricky Fung"
    $ git config [--global] user.email ricky_feng@163.com
```
如果使用了 –global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 –global 选项的命令来配置。

#### 查看配置信息

如果想要检查你的配置，可以使用 git config –list 命令来列出所有 Git 当时能找到的配置。
```bash
    $ git config --list
    core.symlinks=false
    core.autocrlf=false
    color.diff=auto
    color.status=auto
    color.branch=auto
    color.interactive=true
    help.format=html
    http.sslcainfo=C:/Users/Ricky/AppData/Local/Programs/Git/mingw64/ssl/certs/ca-bundle.crt
    diff.astextplain.textconv=astextplain
    rebase.autosquash=true
    user.name=Ricky Fung
    user.email=ricky_feng@163.com
```

### 查看当前文件状态

要查看哪些文件处于什么状态，可以用 git status 命令。 如果在克隆仓库后立即使用此命令，会看到类似这样的输出：
```bash
    $ git status
    On branch master
    nothing to commit, working directory clean
```

### 添加文件
```bash
    $ git add [file1] [file2] ...
    $ git add [dir]
    $ git add .
    $ git add -A
```

### 删除文件
```bash
    $ git rm [file1] [file2] ...
```

### 代码提交
```bash
    $ git commit -m [message]  如: git commit -m "modify"
    $ git commit [file1] [file2] ... -m [message]
    $ git commit -a
    $ git commit -v
    $ git commit --amend -m [message]
    $ git commit --amend [file1] [file2] ...
```

### 标签
```bash
    $ git tag
    $ git tag [tag name]
    $ git tag -a [tag name] -m [message]
    $ git tag -a [tag name] [version]
    $ git tag -d [tag]
    $ git push origin :refs/tags/[tagname]
    $ git push origin --delete tag 
    $ git show [tag]
    $ git push [remote] [tag]
    $ git push [remote] --tags
    $ git checkout -b [branch] [tag]
```

### 分支管理
```bash
    $ git branch
    $ git branch -r
    $ git branch -a
    $ git branch [branch-name]
    $ git checkout -b [branch]
    $ git checkout [branch-name]
    $ git merge [branch]
    $ git branch -d [branch-name]
    $ git push origin --delete [branch-name]
    $ git push origin :[branch-name]
```

### 变基
```bash
    $ git checkout dev
    $ git rebase master
    First, rewinding head to replay your work on top of it...
    Applying: added staged command
```

### 查看log
```bash
    $ git log
```

### diff
```bash
    $ git diff [file]
```

### show
```bash
    $ git show [version]
    $ git show --name-only [version]
    $ git show [version]:[filename]
```

### 远程同步
```bash
    $ git fetch [remote]
    $ git remote -v
    $ git remote show [remote]
    $ git remote add [shortname] [url]
    $ git pull [remote] [branch]
    $ git push [remote] [branch]
```

## 参考资料

《Pro Git 2nd Edition》

  
