---
title: yarn使用文档
comments: true
tags:
  - yarn
  - npm
  - 包管理
abbrlink: 70a6ae5d
date: 2018-01-08 10:23:31
categories:
---

## 新手指南

Yarn 对你的代码来说是一个包管理器， 你可以通过它使用全世界开发者的代码，或者分享自己的代码。 Yarn 做这些快捷、安全、可靠，所以你不用担心什么。

通过Yarn你可以使用其他开发者针对不同问题的解决方案，使自己的开发过程更简单。 使用过程中遇到问题，你可以将其上报或者贡献解决方案。一旦问题被修复，Yarn会更新保持同步。

代码通过包（package）（或者称为模块（module））的方式来共享。 一个包里包含所有需要共享的代码，以及描述包信息的文件，称为<font color=#dc143c background-color=#b0c4de>`package.json`</font>。

<!--more-->

## 安装
> 稳定版：[v1.3.2][01]
> Node.js版本：^4.8.0 || ^5.7.0 || ^6.2.2 || ^8.0.0

在你使用 Yarn 之前，你需要先在系统中安装它。有一些不同的方法（还在增加）安装 Yarn：

### windows 中如何安装
在 Windows 系统中安装 Yarn 有多种方法

- **方法一：直接下载安装程序安装**

这将给你一个.msi 文件([下载安装程序][02])，当你运行它时带领你安装 Yarn 到 Windows 上。

前提，你需要先安装 [Node.js][03]。

- **方法二：用 Chocolatey 安装**

[Chocolatey][04] 是一个适用于 Windows 的包管理器，您可以用这个*[操作方式][05]*安装。

如果你已经安装了 Chocolatey，你可以在你的控制台里运行下面的指令安装 yarn：
```bash
	choco install yarn
```
同样的，你需要确保先安装了 [Node.js][03]。

- **方法三：通过 Scoop 安装**

[Scoop][06] 是一个 Windows 的命令行安装程序，你可以用[这些指令][07]安装 Scoop。

安装好了 Scoop 后，你可以在控制台里运行下面的代码安装 yarn：
```bash
	scoop install yarn
```
如果之前没有安装 [Node.js][03]，scoop 将给你一个建议来安装它。 例如：
```bash
	scoop install nodejs
```

- **方法四(备选)：通过 npm 安装**

> **注意：**一般来说, 不推荐通过 npm 安装 Yarn 在用基于 Node 的包管理器安装 Yarn 时，该包未被签名，并且只通过基本的 SHA1 散列进行唯一完整性检查。 这在安装系统级应用时有安全风险。
> 
> 因为这些原因，高度推荐用你的操作系统最适合的方式来安装 Yarn。

你还可以通过 npm package manager 来安装 Yarn。 如果你已经装了Node.js，那你应该已经有 npm 了。
安装好 npm 后你可以用如下命令来安装 yarn：
```bash
	npm install --global yarn
```
此外你还需要在终端里设置`PATH`环境变量来全局访问 Yarn 的二进制可执行程序。
添加 `set PATH=%PATH%;C:\.yarn\bin` 到你的 shell 环境。

> - **提示**：
请把你的项目目录和 Yarn 缓存目录 (%LocalAppData%\Yarn) 加到你的杀毒软件白名单里，否则安装包时会明显变慢，每个写入到硬盘时将被扫描。

- **测试 Yarn 是否安装成功**

运行如下命令：
```bash
	yarn --version
```

### [Mac OS 中如何安装][12]
### [Debian/Ubuntu Linux 中如何安装][13]

### [Nightly Builds][08]

Nightly Builds 是 Yarn 最新和最大的版本，使用最新的 Yarn 代码构建。Nightly Builds 对于尝试还没有作为稳定版发布的新功能或测试 bug 修复很有用。这些 Builds 不保证稳定并且可能有 bugs。
[请参阅如何安装 Nightly Builds][09]

> 有问题吗？ 如果你不能用这些安装程序安装 Yarn，请通过 GitHub 搜索一个 issue 或者开一个新的 issue。
>
> [搜索现有的 issue][10] · [开一个新的 issue][11]


## 使用

现在Yarn已经 安装完毕，可以开始使用。以下是一些你需要的最常用的命令：

- **初始化新项目**
```bash
	yarn init
```

- **添加依赖包**
```bash
	yarn add [package]
	yarn add [package]@[version]
	yarn add [package]@[tag]
```

- **将依赖项添加到不同依赖项类别**
分别添加到 `devDependencies`、`peerDependencies` 和 `optionalDependencies`：
```bash
	yarn add [package] --dev
	yarn add [package] --peer 
	yarn add [package] --optional
```

- **升级依赖包**
```bash
	yarn upgrade [package]
	yarn upgrade [package]@[version]
	yarn upgrade [package]@[tag]
```

- **移除依赖包**
```bash
	yarn remove [package]
```

- **安装项目的全部依赖**
```bash
	yarn
	或者
	yarn install
```

> 延伸阅读
> [Yarn 工作流(如何使用Yarn？方便的创建与使用Yarn包的工作流程将帮你快速提高生产力。)][14]
> [CLI 命令(Yarn 通过一组丰富的命令执行包安装、管理、发布等操作。)][15]

> [官方文档][16]





[01]: https://github.com/yarnpkg/yarn/releases/tag/v1.3.2
[02]: https://yarnpkg.com/latest.msi
[03]: https://nodejs.org/
[04]: https://chocolatey.org/
[05]: https://chocolatey.org/install
[06]: http://scoop.sh/
[07]: https://github.com/lukesampson/scoop/wiki/Quick-Start
[08]: https://baike.baidu.com/item/Nightly%20Build/9058981
[09]: https://yarnpkg.com/zh-Hans/docs/nightly
[10]: https://github.com/yarnpkg/yarn/issues?utf8=%E2%9C%93&q=is%3Aissue%20is%3Aopen%20%22Installation%20Problem%22%20in%3Atitle%20
[11]: https://github.com/yarnpkg/yarn/issues/new?title=Installation%20Problem:%20[title]&body=%0A**Which%20operating%20system%20are%20you%20using:**%0A%0A%0A**Please%20describe%20the%20steps%20you%20took%20when%20trying%20to%20install%20Yarn%20and%20what%20went%20wrong:**%0A%0A
[12]: https://yarnpkg.com/zh-Hans/docs/install#mac-tab
[13]: https://yarnpkg.com/zh-Hans/docs/install#linux-tab
[14]: https://yarnpkg.com/zh-Hans/docs/yarn-workflow
[15]: https://yarnpkg.com/zh-Hans/docs/cli/
[16]: https://yarnpkg.com/zh-Hans/docs


