---
title: "js事件之event.preventDefault()与event.stopPropagation()用法区别"
comment: true
draft: false
date: 2018-03-28T02:35:37+08:00
lastmod: 2018-03-28T02:35:37+08:00
categories: ["", ""]
tags: ["", "", ""]
author: '<a href="https://laozhu.me" rel="noopener" target="_blank">米老朱</a>'
weight: 10
contentCopyright: '<a href="#" rel="noopener" target="_blank">See origin</a>'
hiddenFromHomePage: true
---

### event.preventDefault()用法介绍

该方法将通知 Web 浏览器不要执行与事件关联的默认动作（如果存在这样的动作）。例如，如果 type 属性是 "submit"，在事件传播的任意阶段可以调用任意的事件句柄，通过调用该方法，可以阻止提交表单。注意，如果 Event 对象的 cancelable 属性是 fasle，那么就没有默认动作，或者不能阻止默认动作。无论哪种情况，调用该方法都没有作用。

该方法将通知 Web 浏览器不要执行与事件关联的默认动作（如果存在这样的动作）。

例如，如果 type 属性是 "submit"，在事件传播的任意阶段可以调用任意的事件句柄，通过调用该方法，可以阻止提交表单。

注意，如果 Event 对象的 cancelable 属性是 fasle，那么就没有默认动作，或者不能阻止默认动作。无论哪种情况，调用该方法都没有作用。

 

### event.stopPropagation()用法介绍

该方法将停止事件的传播，阻止它被分派到其他 Document 节点。在事件传播的任何阶段都可以调用它。注意，虽然该方法不能阻止同一个 Document 节点上的其他事件句柄被调用，但是它可以阻止把事件分派到其他节点

该方法将停止事件的传播，阻止它被分派到其他 Document 节点。在事件传播的任何阶段都可以调用它。

注意:虽然该方法不能阻止同一个 Document 节点上的其他事件句柄被调用，但是它可以阻止把事件分派到其他节点。

event是DOM的事件方法，所以不是单独使用，比如指定DOM





### DOM

1.元素节点有tagName 、nodeName 、localName属性；其中tagName 、nodeName相同，都是大写，localName是小写；

   其他节点只有nodeName 、localName属性，其中属性节点localName和nodeName相同，文本节点localName为null；

2.childNodes是指元素的所有直接子节点，包括元素节点、文本节点，不包括属性节点

   children 返回元素的所有直接子的元素节点

   二者区别在于后者不包括文本节点

   childElementCount 表示的是子元素节点的个数，等于children的length；

3.attibutes  存有html属性，包括class、id等

   properties 

4.获取元素节点内的所有文本值

  innerText      IE和chrome支持

  textContent   高版本浏览器支持

5.outerHTML 与 innerHTML的区别在与是否包换元素本身，包括就是outerHtml;