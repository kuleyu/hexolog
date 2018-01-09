---
title: Web_Maker——一款基于浏览器的可离线的CodePen替代品
comments: true
categories: 工具
tags:
  - web maker
  - 编辑器
  - Chrome插件
  - living preview
abbrlink: 3aa76942
date: 2017-03-18 23:39:31
---

译自: [Web Maker, an Offline, Browser-based CodePen Alternative][01] ——by [Kushagra Gaur][02]
![img1][03]
本文，Kushagra Gaur介绍了一款他本人专为那些需要一个响应迅速且可离线工作的Web平台编译器的前端开发者而制作的浏览器扩展插件 —— [Web Maker][05]。

## 前言

如果你像我一样也是一位前端开发人员，你可能试用过一个或多个代码编译器像 [CodePen][06]、 [JSBin][07]、 [JSFiddle][08] 等（我们国内有 [JSRun][09]、[RunJs][10]）。这些都很好，能完美的满足我们的工作需求。我主要用他们来分析解决我所遇到的问题，或者用来与同事讨论的一些代码片段。但使用这些工具必须要连网，这样连网加载这些应用时就总会有固有延迟。而这也总使我感到不爽。
<!--more-->
当外出旅行或在机场等机时，我可能也需要一个快速的途径来编译或调试代码，然而这种场景下是没有网络的，也就意味着之前提到的那些在线编译器都不能用。那该在呢么办呢？当然，你可以使用编辑器，并在浏览器中查看效果—但在这个快节奏的世界，这样操作就显得有些慢了！

我试着去寻找一款能满足我这种需求的编译器，然而一款也没找着。另外，我发现许多人同我一样也在寻找着这样一个东西︰ [https://twitter.com/armstrong/status/567403700713717763][11] 于是决定自己来解决这个问题，然后就开发出了 [Web Maker][05]。现在我所有的 web 开发项目都会用 [Web Maker][05]，甚至在维护与再开发 [Web Maker][05]本身时也是如此！
![img1][04]

## Web Maker 是什么 ？

Web Maker 是一款 [Chrome 扩展插件][12]，将你浏览器的新选项卡 （可选） 转换成 web 编译器，你可以在其中编写 HTML、CSS 和 JavaScript，同时还可以实时预览页面效果。这款插件已经有上千(现在为4万5千多)用户了，在 [Chrome Web Store][12]上可提供下载。
**下载地址：**[Chrome Web Store][12]（需要翻墙。翻不了墙的也可从[Github][13]中下载后采取[这种方法][14]安装。注：步骤4对应下载包里的src文件夹）。

## 特点

### 速度极快，可离线工作

作为一款 chrome 扩展插件， Web Maker 完全寄生在你的浏览器中。它没有涉及到网络需求（除非你正在使用一个第三方的 JavaScript 和 CSS 库）。所以它初始启动迅速。不仅如此，而且你对代码所做的的每一次中每次变动都能在预览中即时自动刷新。当然，如果仅仅只是 CSS 的变动，它甚至连刷新不需要就直接显示出来。

你还可以选择保存或者加载你编辑好的项目以便以后再次编辑。它们会被保存在您的浏览器的本地存储中

### 预处理器支持

预处理器是几乎每个开发人员的工具链中的一个组成部分。Web Maker 为你提供了HTML、CSS 以及 JavaScript三种语言中所有最常用的预处理器，包括 Markdown、Jade、SCSS、Less、JSX 以及 TypeScript。

如果您需要在你的项目中使用外部的 JavaScript 或 CSS 库 （如 jQuery 或 Bootstrap），你只需简单点击一下”Add Library”按钮，从可用列表中，选择其中一个最受欢迎的库或输入任何库的名称，从显示的自动建议项中选择它即可。

### 多种布局

除了有多个编辑器布局选项，你所保存的每个项目都记得其上次使用的布局选项，以及三个代码编辑窗口的大小。所以，基本上，每当你重新加载之前的任何项目时，你都会得到与你上次保存时完全相同的编辑器配置。

此外，要想在实际的浏览器窗口大小下查看你的项目，只需切换到全屏布局模式即可。

### 预览区域截屏功能

Chrome 扩展的 API 赋予了 Web Maker 强大的能力去做那些普通的 web 应用难以实现的功能。比如说截图捕获功能，只需单击一下 Take Screenshot 按钮即可随时得到预览区域的截图。

### 可另存为 HTML 或在 CodePen 中打开

在 Web Maker 中完成了你的项目后，如果想在其他地方使用它，你并不需要将它复制粘贴到一些文件中，你只需点击 Save as HTML file 选项即可将你项目中的 HTML、 CSS 和 JavaScript 代码嵌入到一个 HTML 文件中。

或者说你想要与世界分享你的项目︰Open on CodePen 按钮可以在 CodePen 中打开你的项目。

### 源代码开源

我（Kushagra Gaur）已经将 Web Maker 在 [GitHub][13] 上开源了，我认为这样可以让我接触到更多的用户，他们可以根据他们的需求提出各种建议或标记出他们所遇到的问题，这样就可以更集中地将问题反馈上来。

在打造这样一款 Web 平台编译器时用到了许多有趣的逻辑块。所有这些都是从开源项目中借鉴的。我个人工作中喜欢使用 Esprima 来预防无限循环。

Web Maker 中大量地使用了一些值得膜拜的开源项目如: [CodeMirror][15]、[Esprima][16]、[Split.js][17]、[Escodegen][18]、[Inlet.js][19]、[Emmet][20] 等。谨此向这些开源项目的作者们表示感谢！如前所述，Web Maker 在开发过程中也使用了 Web Maker。

## 您可以使用 Web Maker 来做什么

除了用于通常的 web 开发工作以外，Web Maker 可以用到很多有趣的方面。让我们来看看其中的一些吧。

**学习的过程中实践，省去安装的麻烦**
如果你正在开始学习 web 开发，Web Maker 是一个你日常练习、作业等的好地方。你可以专注于编写代码而无需分心去设置编辑器或者使用预处理器时代码的生成过程

**为您的应用程序创建独立的组件**
近期，[基于组件的体系结构][21]正被广泛的用于 web 应用程序的设计中。无论你们正在使用 react、Vue 还是其他某个 JavaScript 框架，每个人都朝着使各自的应用程序成为独立组件的集合的方向设计。

您可以在 Web Maker 中开发或者快速地试用这些独立的组件，— — 喜欢的话，也可以将它们集成到你的应用程序中。

**作为一款 Markdown 编辑器**
Web Maker 并不仅仅限于 web 开发。如今，很多博主通常都用 Markdown 来写博客或文章，以至于他们经常要用到 Markdown 编辑器。你可以将 Web Maker 变成一款 Markdown 编辑器，并且可以非常快速地实时预览。（这篇文章是用 Web Maker 写的）。
![img3][22]

**作为在课堂里教学生的工具**
由于 Web Maker 可以离线运行，因而它又是一个很好的平台可以在课堂上供学生们探索实践巩固他们所学到的新东西。

**（可以运行代码片段）调试时帮助减少测试项**
当你试图查找你应用程序中的某个 bug 时，有必要隔离可疑组件，这样你就可以在一个更小的环境中进行调试，不受应用程序中其余部分的任何干扰。 Web Maker 就是这样一个可以快速运行一段代码的很好的工具。

**存储你最喜爱的代码片段**
在网站上找到了一些有趣的代码段时，你不必记住或记下该网页地址，只需打开 Web Maker ，将代码片段粘贴到相应区域，然后给它取个名称并保存即可。这样以后需要参考或编辑时你只要再打开就可以了。

## 即将到来的一些新功能

这些是 Web Maker 一些新功能（已经在新版2.4.0中实现）︰
> **导入/导出**。很快就会有选项可以导出你的作品，而且它们也可以再次导入到 Web Maker。你还能够创建备份到云端（如 Google Drive 的服务）。

> **自定义编辑器**。更多的自定义设置是也在准备中，其中包括能够设置字体大小、 主题和缩进。

>**快捷键** 快捷键支持。
更多详细信息请参考 [GitHub issues][23] 页面中的 roadmap（版本更新线路图）。




[01]: https://www.sitepoint.com/web-maker-an-offline-browser-based-codepen-alternative/
[02]: https://github.com/chinchang/
[03]: http://opifddwc7.bkt.clouddn.com/18-1-7/19949593.jpg
[04]: http://opifddwc7.bkt.clouddn.com/18-1-7/79295989.jpg
[05]: https://webmakerapp.com/
[06]: https://codepen.io/
[07]: http://jsbin.com/
[08]: https://jsfiddle.net/
[09]: http://jsrun.net/
[10]: http://runjs.cn/
[11]: https://twitter.com/armstrong/status/567403700713717763
[12]: https://chrome.google.com/webstore/detail/web-maker/lkfkkhfhhdkiemehlpkgjeojomhpccnh?utm_source=chrome-ntp-icon
[13]: https://github.com/chinchang/web-maker
[14]: https://github.com/kuleyu/IFE-Tasks/issues/1
[15]: https://codemirror.net/
[16]: http://esprima.org/
[17]: http://esprima.org/
[18]: https://github.com/estools/escodegen
[19]: https://github.com/enjalot/Inlet
[20]: http://emmet.io/
[21]: https://medium.com/@dan.shapiro1210/understanding-component-based-architecture-3ff48ec0c238#.je0xb7sbk
[22]: http://opifddwc7.bkt.clouddn.com/18-1-7/98900803.jpg
[23]: https://github.com/chinchang/web-maker/issues

