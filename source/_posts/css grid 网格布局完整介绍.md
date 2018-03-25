---
title: "css grid 网格布局完整介绍"
comments: true
categories: ["搬运整理"]
tags: ["css","grid"]
abbrlink: 30c1a0ec
date: 2018-01-29 12:38:41
---

> 英文原文：[A Complete Guide to Grid][01],感谢[zhaozhiming][02]的翻译。

## 介绍

CSS Grid 布局（又叫“Grid”），是一个基于网格的二维布局系统，目的是为了完全改变我们基于网格设计用户界面的方式。CSS 可以用来做我们的网页布局，但它在这一方面做的不是很好。开始的时候我们使用`tables`, 然后使用`floats`，`positioning`和`inline-block`，但这些方法本质上都是 hack 的方法并缺少一些重要功能（比如`垂直居中`）。`Flexbox`帮助我们解决了问题，但它是简单的一维布局，而不是复杂的二维布局（实际上 Flexbox 和 Grid 可以很好地组合起来使用）。Grid 是第一个专门为了解决那些我们一直使用 hack 手段而导致的页面布局问题而创建的 CSS 模块。
<!-- more -->

我写这篇文章主要收到两个事情启发，第一个是`Rachel Andrew`写的一本好书——《[Get Ready for CSS Grid Layout][03]》，这本书把对 Grid 全面而清晰的介绍作为全书的基调，我高度推荐大家去买这本书来读一下。我另外一件受启发的事情是`Chris Coyier`对 [Flexbox 的完整介绍][04]，这是我推荐学习 flexbox 的首选资源，它帮助了很多人，当你用 Google 搜索 flexbox 时可以从它的搜索结果看出其影响范围。你可以看到那篇文章跟我的文章有很多相似的地方，因为我这篇文章就是通过模仿那篇最好的文章来写的。（译者注：可以看到这两篇文章都是按照两列分布的方式来介绍 flexbox 和 Grid。）

我这篇文章的目的是为了介绍 Grid 在最新规范中的概念，所以我不会涵盖过时的 IE 语法，并且当规范更新时我将尽力更新这篇文章。

## 基础和浏览器支持

开始使用 Grid 非常简单，你只需要通过`display: grid`来定义一个容器元素作为网格，再通过`grid-template-columns`和`grid-templaet-rows`设置列和行的大小，然后通过`grid-column`和`grid-row`来设置网格的子元素，grid 元素的顺序对其实现的效果没有任何影响。你的 CSS 可以任意调节它们的顺序，这可以让你很方便地在媒体查询中重新编排你的网格。想象一下在你的整个页面中定义了一个布局，然后通过几行 CSS 代码就可以重新编排出另外一个布局来适应另外一个屏幕，所以说 Grid 是有史以来最强大一个的 CSS 模块。

~~理解 Grid 最重要的一件事情是现在还不能把它用在生产环境。它现在还只是一个 W3C 的在制品草稿，还没有任何浏览器默认是支持它的。IE10 和 11 可以支持它，但它们是用过时的语法做的一个老旧的实现。最好地使用 Grid 的方式是设置 Chrome，Opera 或者 Firefox 的特殊标志来启用它。在 Chrome 中，在地址栏输入`chrome://flags`然后将`experimental web platform features`选项设置为`enable`，这个方法同样适用于 Opera(`opera://flags`)，在 Firefox 中，将`layout.css.grid`选项设置为可用。~~

截至2017年3月，许多浏览器都提供了原生的、无需前缀的CSS Grid支持：Chrome（包括Android），Firefox，Safari（包括iOS）、Opera 和 Edge。另一方面，Internet Explorer 10和11支持它，但是它用的是一个过时的语法实现的。

这是一个支持的浏览器表格，详情见[caniuse.com][05]：

<table class="browser-support-table">
<thead>
<tr>
<th class="chrome"><span>Chrome</span></th>
<th class="safari"><span>Safari</span></th>
<th class="firefox"><span>Firefox</span></th>
<th class="opera"><span>Opera</span></th>
<th class="ie"><span>IE</span></th>
<th class="android"><span>Android</span></th>
<th class="iOS"><span>iOS</span></th>
</tr>
</thead>
<tbody>
<tr>
<td class="yep" data-browser-name="Chrome">29+ (Behind flag)</td>
<td class="nope" data-browser-name="Safari">10.1+</td>
<td class="yep" data-browser-name="Firefox">40+ (Behind flag)</td>
<td class="yep" data-browser-name="Opera">28+ (Behind flag)</td>
<td class="yep" data-browser-name="IE">10+ (Old syntax)</td>
<td class="nope" data-browser-name="Android">62+</td>
<td class="nope" data-browser-name="iOS">10.3+</td>
</tr>
</tbody>
</table>

~~除了微软，其他浏览器好像不想太早实现 Grid 直到规范完全成熟为止，这是一件好事，这意味着我们不用担心以后使用 Grid 要使用多种语法。
在生产环境使用 Grid 只是时间上的问题，但现在是时候可以学习它了。~~

## 重要的术语

在开始了解 Grid 的概念之前先理解其相关的术语是很重要的，因为这里涉及的概念都有点相似，所以如果你不记住它们在规范中的定义的话会很容易被搞混，但请不用担心，这里的术语并不多。

{% img http://opifddwc7.bkt.clouddn.com/18-1-29/24226299.jpg %}

### 网格容器（grid container）

网格容器是指这个元素使用了`display: grid`，它是所有网格元素的直接父级，在这个例子`container`的元素就是网格容器。

{% codeblock lang:html %}
<div class="container">
  <div class="item item-1"></div>
  <div class="item item-2"></div>
  <div class="item item-3"></div>
</div>
{% endcodeblock %}

### 网格项（grid item）

网格项是指网格容器的子元素（比如其直接后代），在下面的例子中`item`的元素是网格项，但`sub-item`的元素不是。

{% codeblock lang:html %}
<div class="container">
  <div class="item"></div>
  <div class="item">
    <p class="sub-item"></p>
  </div>
  <div class="item"></div>
</div>
{% endcodeblock %}

### 网格线（grid line）

分隔的线组成了网格的结构。它们可以是垂直的（“列网格线”）或者水平的（“行网格线”），也可以在行或列的任一边。下面的例子中黄色的线是一个列网格线的例子。

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-line.png %}

### 网格轨道（grid track）

网格轨道是指两根毗邻网格线中间的位置，你可以认为是网格的行或者列。下面例子的中网格轨道是第二和第三行网格线中间的位置。

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-track.png %}

### 网格单元（grid cell）

网格单元是指两根毗邻的行网格线和列网格线中间的位置，它是一个单独的网格“单元”，下面的例子中网格单元是指第 1 和 2 根行网格线和第 2 和 3 根列网格线中间的位置。

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-cell.png %}

### 网格区域（grid area）

网格区域是指 4 根网格线包围的空间，一个网格空间可能由任意数量的网格单元构成。下面的例子中网格区域是指在第 1 和 3 的行网格线和第 1 和 3 列网格线中间的位置。

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-area.png %}

## 网格容器的属性

* [display](#display)
* [grid-template-columns](#grid-template-columns与grid-template-rows)
* [grid-template-rows](#grid-template-columns与grid-template-rows)
* [grid-template-areas](#grid-template-areas)
* [grid-column-gap](#grid-column-gap)
* [grid-row-gap](#grid-row-gap)
* [grid-gap](#grid-gap)
* [justify-items](#justify-items)
* [align-items](#align-items)
* [justify-content](#justify-content)
* [align-content](#align-content)
* [grid-auto-columns](#grid-auto-columns)
* [grid-auto-rows](#grid-auto-rows)
* [grid-auto-flow](#grid-auto-flow)
* [grid](#grid)

### display

定义一个元素为网格容器并为其内容创建一个新的网格格式环境。

值有：

* **grid** - 生成一个块级别的网格
* **inline-grid** - 生成一个内联级别的网格
* **subgrid** - 如果你的网格容器是它自己的一个网格子项（比如内嵌的网格），你可以使用这个属性来表示你想要从其父级来获取行和列的大小而不是自己来指定它们。

{% codeblock lang:css %}
.container{
  display: grid | inline-grid | subgrid;
}
{% endcodeblock %}

注意：`column`, `float`, `clear`和`vertical-align`对网格容器没有效果。

### grid-template-columns与grid-template-rows

通过空格分隔的一系列值来定义网格的行和列，这些值相当于轨迹大小，它们之间的距离相当于网格线。

值有：

* **\<track-size\>** - 可以是一个长度，百分比或者是网格中自由空间的份数（使用`fr`这个单位）
* **\<line-name\>** - 一个你选择的任意名字

{% codeblock lang:css %}
.container{
  grid-template-columns: <track-size> ... | <line-name> <track-size> ...;
  grid-template-rows: <track-size> ... | <line-name> <track-size> ...;
}
{% endcodeblock %}

举例：

当你在轨迹值中间留空格，网格线将被自动以数字命名：

{% codeblock lang:css %}
.container{
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-numbers.png %}

但你可以给网格线指定一个名字，注意网格线命名时的中括号语法：

{% codeblock lang:css %}
.container{
  grid-template-columns: [first] 40px [line2] 50px [line3] auto [col4-start] 50px [five] 40px [end];
  grid-template-rows: [row1-start] 25% [row1-end] 100px [third-line] auto [last-line];
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-names.png %}

注意一根网格线可以有多个名字，例如在下面的例子中第二根线有两个名字：`row1-end` 和 `row2-start`。

{% codeblock lang:css %}
.container{
  grid-template-rows: [row1-start] 25% [row1-end row2-start] 25% [row2-end];
}
{% endcodeblock %}

如果你定义了容器的重复部分，你可以使用`repeat()`方法来生成多个相同值：

{% codeblock lang:css %}
.container{
  grid-template-columns: repeat(3, 20px [col-start]) 5%;
}
{% endcodeblock %}

它等价于：

{% codeblock lang:css %}
.container{
  grid-template-columns: 20px [col-start] 20px [col-start] 20px [col-start] 5%;
}
{% endcodeblock %}

`fr`单位允许你将网格容器中的自由空间设置为一个份数，举个例子，下面的例子将把网格容器的每个子项设置为三分之一。

{% codeblock lang:css %}
.container{
  grid-template-columns: 1fr 1fr 1fr;
}
{% endcodeblock %}

自由空间是在所有固定项确定后开始计算的，在下面的例子中自由空间是`fr`单位的总和但不包括`50px`：

{% codeblock lang:css %}
.container{
  grid-template-columns: 1fr 50px 1fr 1fr;
}
{% endcodeblock %}

### grid-template-areas

通过引用在`grid-area`属性中指定的网格区域名字来定义网格模板。重复网格区域的名字将让内容跨越那些单元。一个句点表示一个空单元，语法本身提供了一个可视化的结构网格。

值有：

* **\<grid-area-name\>** - 在`grid-area`中指定的网格区域名字
* **.** - 一个句点表示一个空的网格单元
* **none** - 没有网格区域被定义

{% codeblock lang:css %}
.container{
  grid-template-areas: "<grid-area-name> | . | none | ..."
                       "..."
}
{% endcodeblock %}

举个例子：

{% codeblock lang:css %}
.item-a{
  grid-area: header;
}
.item-b{
  grid-area: main;
}
.item-c{
  grid-area: sidebar;
}
.item-d{
  grid-area: footer;
}

.container{
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: auto;
  grid-template-areas: "header header header header"
                       "main main . sidebar"
                       "footer footer footer footer"
}
{% endcodeblock %}

这将创建一个 4 乘以 3 的网格，第一行由`header`区域组成，中间一行由 2 个`main`区域和一个空单元和一个`sidebar`区域组成，最后一行由`footer`区域组成。

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-template-areas.png %}

在你定义的每一行都需要拥有相同的单元格。

你可以使用任意毗邻的阶段来声明一个单独的空单元，只要这些阶段中间没有空间都可以认为是一个单独的单元。

注意，在这里你的语法只是命名了区域但没有对网格线进行命名，当你使这种语法时，区域任意一边的线会被自动命名。如果你的网格区域的名字是`foo`，然么网格的开始行和开始列网格线的名字将会是`foo-start`，并且它的最后一行和最后一列的网格线名字是`foo-end`。这意味着一些网格线可能有多个名字，比如上面那个例子中最左边的线，它会有三个名字分别是：`header-start`，`main-start`，`footer-start`。

### grid-column-gap
### grid-row-gap

指定网格线的大小，你可以认为它就是设置行和列中间沟槽的宽度。

值有：

* **\<line-size\>** - 一个长度值

{% codeblock lang:css %}
.container{
  grid-column-gap: <line-size>;
  grid-row-gap: <line-size>;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.container{
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px;
  grid-column-gap: 10px;
  grid-row-gap: 15px;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-column-row-gap.png %}

只会创建行和列的沟槽，不包括边缘。

### grid-gap

一个`grid-column-gap` + `grid-row-gap`的简称。

值有：

* **\<grid-column-gap\>**、**\<grid-row-gap\>** - 长度值

{% codeblock lang:css %}
.container{
  grid-gap: <grid-column-gap> <grid-row-gap>;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.container{
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px;
  grid-gap: 10px 15px;
}
{% endcodeblock %}

如果没有写`grid-row-gap`，那么它的值将和`grid-column-gap`的一样。

### justify-items

让网格子项的内容和列轴对齐（`align-items`则相反，是和行轴对齐），这个值对容器里面的所有网格子项都有用。

值有：

* **start** - 内容和网格区域的左边对齐
* **end** - 内容和网格区域的右边对齐
* **center** - 内容和网格区域的中间对齐
* **stretch** - 填充整个网格区域的宽度（默认值）

{% codeblock lang:css %}
.container{
  justify-items: start | end | center | stretch;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.container{
  justify-items: start;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-items-start.png %}

{% codeblock lang:css %}
.container{
  justify-items: end;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-items-end.png %}

{% codeblock lang:css %}
.container{
  justify-items: center;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-items-center.png %}

{% codeblock lang:css %}
.container{
  justify-items: stretch;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-items-stretch.png %}

可以通过`justify-self`属性把这个行为设置到单独的网格子项。

### align-items

让网格子项的内容和行轴对齐（`justify-items`则相反，是和列轴对齐），这个值对容器里面的所有网格子项都有用。

值有：

* **start** - 内容和网格区域的上边对齐
* **end** - 内容和网格区域的下边对齐
* **center** - 内容和网格区域的中间对齐
* **stretch** - 填充整个网格区域的高度（默认值）

{% codeblock lang:css %}
.container{
  align-items: start | end | center | stretch;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.container{
  align-items: start;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-items-start.png %}

{% codeblock lang:css %}
.container{
  align-items: end;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-items-end.png %}

{% codeblock lang:css %}
.container{
  align-items: center;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-items-center.png %}

{% codeblock lang:css %}
.container{
  align-items: stretch;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-items-stretch.png %}

可以通过`align-self`属性把这个行为设置到单独的网格子项。

### justify-content

有时候你的网格总大小可能会比它的网格容器的容量小，这可能是你的所有网格子项都使用了固定值比如`px`来确定大小，在这个情况下你可以在网格容器中设置网格的对齐方式。这个属性将网格和列轴对齐（和`align-content`相反，它是和行轴对齐）。

值有：

* **start** - 网格在网格容器左边对齐
* **end** - 网格在网格容器右边对齐
* **center** - 网格在网格容器中间对齐
* **stretch** - 改变网格子项的容量让其填充整个网格容器宽度
* **space-around** - 在每个网格子项中间放置均等的空间，在始末两端只有一半大小
* **space-between** - 在每个网格子项中间放置均等的空间，在始末两端没有空间
* **space-evenly** - 在每个网格子项中间放置均等的空间，包括始末两端

{% codeblock lang:css %}
.container{
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.container{
  justify-content: start;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-content-start.png %}

{% codeblock lang:css %}
.container{
  justify-content: end;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-content-end.png %}

{% codeblock lang:css %}
.container{
  justify-content: center;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-content-center.png %}

{% codeblock lang:css %}
.container{
  justify-content: stretch;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-content-stretch.png %}

{% codeblock lang:css %}
.container{
  justify-content: space-around;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-content-space-around.png %}

{% codeblock lang:css %}
.container{
  justify-content: space-between;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-content-space-between.png %}

{% codeblock lang:css %}
.container{
  justify-content: space-evenly;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-content-space-evenly.png %}

### align-content

有时候你的网格总大小可能会比它的网格容器的容量小，这可能是你的所有网格子项都使用了固定值比如`px`来确定大小，在这个情况下你可以在网格容器中设置网格的对齐方式。这个属性将网格和行轴对齐（和`justify-content`相反，它是和列轴对齐）。

值有：

* **start** - 网格在网格容器上边对齐
* **end** - 网格在网格容器下边对齐
* **center** - 网格在网格容器中间对齐
* **stretch** - 改变网格子项的容量让其填充整个网格容器高度
* **space-around** - 在每个网格子项中间放置均等的空间，在始末两端只有一半大小
* **space-between** - 在每个网格子项中间放置均等的空间，在始末两端没有空间
* **space-evenly** - 在每个网格子项中间放置均等的空间，包括始末两端

{% codeblock lang:css %}
.container{
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;
}
{% endcodeblock %}

{% codeblock lang:css %}
.container{
  align-content: start;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-content-start.png %}

{% codeblock lang:css %}
.container{
  align-content: end;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-content-end.png %}

{% codeblock lang:css %}
.container{
  align-content: center;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-content-center.png %}

{% codeblock lang:css %}
.container{
  align-content: stretch;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-content-stretch.png %}

{% codeblock lang:css %}
.container{
  align-content: space-around;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-content-space-around.png %}

{% codeblock lang:css %}
.container{
  align-content: space-between;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-content-space-between.png %}

{% codeblock lang:css %}
.container{
  align-content: space-evenly;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-content-space-evenly.png %}

### grid-auto-columns
### grid-auto-rows

指定自动生成的网格轨道的大小（又叫隐式网格轨道），当你精确指定行和列的位置大于定义的网格（通过 grid-template-rows/grid-template-columns）时隐式网格轨道会被创建。

值有：

* **\<track-size\>** - 可以是一个长度，百分比或者是一个网格中自由空间的份数（通过使用`fr`单位）。

{% codeblock lang:css %}
.container{
  grid-auto-columns: <track-size> ...;
  grid-auto-rows: <track-size> ...;
}
{% endcodeblock %}

为了说明隐式网格轨道如何被创建，思考一下这个：

{% codeblock lang:css %}
.container{
  grid-template-columns: 60px 60px;
  grid-template-rows: 90px 90px
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-auto.png %}

这里创建了 2 x 2 的网格。

但现在想象你使用`grid-column`和`grid-row`来定位你的网格项，就像这样：

{% codeblock lang:css %}
.item-a{
  grid-column: 1 / 2;
  grid-row: 2 / 3;
}
.item-b{
  grid-column: 5 / 6;
  grid-row: 2 / 3;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/implicit-tracks.png %}

我们告诉`.item-b`在第 5 列网格线开始第 6 列网格线结束，但我们还没有定义第 5 或者第 6 列。因为我们引用的线不存在，0 宽度的隐式网格轨道将被创建来填充这些空缺。我们可以使用`grid-auto-columns`和`grid-auto-rows`来指定这些隐式网格轨道的宽度：

{% codeblock lang:css %}
.container{
  grid-auto-columns: 60px;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/implicit-tracks-with-widths.png %}

### grid-auto-flow

如果你有网格项没有明确地放置在网格中，自动布局算法会将网格项自动放置起来，这个属性控制自动布局算法如何工作。

值有：

* **row** - 告诉自动布局算法在每一行中依次填充，必要时添加新行
* **column** - 告诉自动布局算法在每一列中依次填充，必要时添加新列
* **dense** - 告诉自动布局算法如果更小的子项出现时尝试在网格中填补漏洞

{% codeblock lang:css %}
.container{
  grid-auto-flow: row | column | row dense | column dense
}
{% endcodeblock %}

注意`dense`可能让你的网格子项出现错乱。

举个例子：

考虑一下这个 HTML：

{% codeblock lang:html %}
<section class="container">
    <div class="item-a">item-a</div>
    <div class="item-b">item-b</div>
    <div class="item-c">item-c</div>
    <div class="item-d">item-d</div>
    <div class="item-e">item-e</div>
</section>
{% endcodeblock %}

你定义一个 5 列 2 行的网格，并设置`grid-auto-flow`为`row`（这也是默认值）：

{% codeblock lang:css %}
.container{
    display: grid;
    grid-template-columns: 60px 60px 60px 60px 60px;
    grid-template-rows: 30px 30px;
    grid-auto-flow: row;
}
{% endcodeblock %}

当在网格中放置子项时，你只能为其中 2 个指定斑点：

{% codeblock lang:css %}
.item-a{
    grid-column: 1;
    grid-row: 1 / 3;
}
.item-e{
    grid-column: 5;
    grid-row: 1 / 3;
}
{% endcodeblock %}

因为我们设置`grid-auto-flow`为`row`，我们的网格看起来就像这样，注意这三个我们没有放置的子项（`item-b`，`item-c`，`item-d`）将如何以行的方式流动的：

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-auto-flow-row.png %}

如果我们将`grid-auto-flow`设为`column`，`item-b`，`item-c `和`item-d`以列的方式向下流动：

{% codeblock lang:css %}
.container{
    display: grid;
    grid-template-columns: 60px 60px 60px 60px 60px;
    grid-template-rows: 30px 30px;
    grid-auto-flow: column;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-auto-flow-column.png %}

### grid

以下属性的简写方式：`grid-template-rows`，`grid-template-columns`，`grid-template-areas`，`grid-auto-rows`，`grid-auto-columns`，`grid-auto-flow`。它也可以设置`grid-column-gap`和`grid-row-gap`为它们的初始值，尽管它们不能通过这个属性来精确设置。
值有：

* **none** - 设置所有子属性为它们的初始值
* **\<grid-template-rows\>** / **\<grid-template-columns\>** - 分别设置`grid-template-rows`和`grid-template-columns`的指定值，以及设置其他所有子属性为初始值
* **\<grid-auto-flow\>** [**\<grid-auto-rows\>** [ / **\<grid-auto-columns\>**] ] - 分别接收所有像`grid-auto-flow`，`grid-auto-rows`和`grid-auto-columnsaccepts`的相同值。如果`grid-auto-columns`被省略了，那么它的值会通过`grid-auto-rows`来设置，如果两个都省略了，它们将被设置为默认值。

{% codeblock lang:css %}
.container{
    grid: none | <grid-template-rows> / <grid-template-columns> | <grid-auto-flow> [<grid-auto-rows> [/ <grid-auto-columns>]];
}
{% endcodeblock %}

举例：

下面 2 段代码是相等的：

{% codeblock lang:css %}
.container{
    grid: 200px auto / 1fr auto 1fr;
}
{% endcodeblock %}

{% codeblock lang:css %}
.container{
    grid-template-rows: 200px auto;
    grid-template-columns: 1fr auto 1fr;
    grid-template-areas: none;
}
{% endcodeblock %}

下面这 2 段代码也是等价的：

{% codeblock lang:css %}
.container{
    grid: column 1fr / auto;
}
{% endcodeblock %}

{% codeblock lang:css %}
.container{
    grid-auto-flow: column;
    grid-auto-rows: 1fr;
    grid-auto-columns: auto;
}
{% endcodeblock %}

它也可以接收一个更复杂但又相当方便的语法来一次性设置所有属性，你可以指定`grid-template-areas`，`grid-auto-rows`和`grid-auto-columns`，并且所有其他子属性被设置为它们的默认值。你需要做的是指定网格线的名称和网格轨迹的大小来生成它们的网格区域。最简单的表述方法就是举一个例子：

{% codeblock lang:css %}
.container{
    grid: [row1-start] "header header header" 1fr [row1-end]
          [row2-start] "footer footer footer" 25px [row2-end]
          / auto 50px auto;
}
{% endcodeblock %}

上面跟下面是等价的：

{% codeblock lang:css %}
.container{
    grid-template-areas: "header header header"
                         "footer footer footer";
    grid-template-rows: [row1-start] 1fr [row1-end row2-start] 25px [row2-end];
    grid-template-columns: auto 50px auto;
}
{% endcodeblock %}

## 网格项的属性

* grid-column-start
* grid-column-end
* grid-row-start
* grid-row-end
* [grid-column](#grid-column与grid-row)
* [grid-row](#grid-column与grid-row)
* [grid-area](#grid-area)
* [justify-self](#justify-self)
* [align-self](#align-self)

### grid-column-start、grid-column-end、grid-row-start 和 grid-row-end
  
通过参考指定的网格线来决定网格中一个网格项的位置，`grid-column-start/grid-row-start`是指网格子项开始的线，`grid-column-end/grid-row-end`是指网格项结束的线。

值有：

* `<line>` - 可以是一个数字以适用被标记了数字号的网格线，或者是一个名字以适用命名了的网格线
* span `<number>` - 子项将跨越指定数字的网格轨道
* span `<name>` - 子项将跨越到指定名字之前的网格线
* auto - 表示自动布局，自动跨越或者默认跨越一个

{% codeblock lang:css %}
.item{
  grid-column-start: <number> | <name> | span <number> | span <name> | auto
  grid-column-end: <number> | <name> | span <number> | span <name> | auto
  grid-row-start: <number> | <name> | span <number> | span <name> | auto
  grid-row-end: <number> | <name> | span <number> | span <name> | auto
}
{% endcodeblock %}

举个例子：

{% codeblock lang:css %}
.item-a{
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start
  grid-row-end: 3
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-start-end-a.png %}

{% codeblock lang:css %}
.item-b{
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2
  grid-row-end: span 2
}
{% endcodeblock %}

{% img http://chris.house/images/grid-start-end-b.png %}

如果`grid-column-end/grid-row-end`没有生命，网格项将默认跨越一个网格轨道。

网格项可以互相重叠，你可以使用`z-index`来控制他们的层叠顺序。

### grid-column与grid-row

`grid-column-start` + `grid-column-end`，和`grid-row-start` + `grid-row-end`的简写，分别独立。

值有：

* <start-line> / <end-line> - 每一个属性都可以接收普通模式的值，包括`span`

{% codeblock lang:css %}
.item{
  grid-column: <start-line> / <end-line> | <start-line> / span <value>;
  grid-row: <start-line> / <end-line> | <start-line> / span <value>;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.item-c{
  grid-column: 3 / span 2;
  grid-row: third-line / 4;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-start-end-c.png %}

如果没有声明结束网格线的值，那么网格子项将默认跨越 1 个网格轨迹。

### grid-area

给网格项取一个名字以让它被由`grid-template-areas`属性创建的模板引用。同时，这个属性也可以用来更简短地表示`grid-row-start`+ `grid-column-start` + `grid-row-end`+ `grid-column-end`。

值有：

* `<name>` - 一个你选择的名字
* `<row-start>` / `<column-start>` / `<row-end>` / `<column-end>` - 可以是网格线的数字或名字

{% codeblock lang:css %}
.item{
  grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
}
{% endcodeblock %}

举例：

作为分配一个名字给网格项的一种方式：

{% codeblock lang:css %}
.item-d{
  grid-area: header
}
{% endcodeblock %}

作为`grid-row-start`+ `grid-column-start` + `grid-row-end`+ `grid-column-end`的一种简写：

{% codeblock lang:css %}
.item-d{
  grid-area: 1 / col4-start / last-line / 6
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-start-end-d.png %}

### justify-self

让网格项的内容以列轴对齐（与之相反`align-self`是跟行轴对齐），这个值可以应用在单个网格项的内容中。

值有：

* **start** - 让内容在网格区域左对齐
* **end** - 让内容在网格区域右对齐
* **center** - 让内容在网格区域中间对齐
* **stretch** - 填充整个网络区域的宽度（默认值）

{% codeblock lang:css %}
.item{
  justify-self: start | end | center | stretch;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.item-a{
  justify-self: start;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-self-start.png %}

{% codeblock lang:css %}
.item-a{
  justify-self: end;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-self-end.png %}

{% codeblock lang:css %}
.item-a{
  justify-self: center;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-self-center.png %}

{% codeblock lang:css %}
.item-a{
  justify-self: stretch;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-justify-self-stretch.png %}

为了让网格中的所有项都对齐，这个行为也可以通过设置网格容器中的`justify-items`属性来实现。

### align-self

让网格项的内容以行轴对齐（与之相反`justify-self`是跟列轴对齐），这个值可以应用在单个网格项的内容中。

值有：

* **start** - 让内容在网格区域上对齐
* **end** - 让内容在网格区域下对齐
* **center** - 让内容在网格区域中间对齐
* **stretch** - 填充着呢个网络区域的高度（默认值）

{% codeblock lang:css %}
.item{
  align-self: start | end | center | stretch;
}
{% endcodeblock %}

举例：

{% codeblock lang:css %}
.item-a{
  align-self: start;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-self-start.png %}

{% codeblock lang:css %}
.item-a{
  align-self: end;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-self-end.png %}

{% codeblock lang:css %}
.item-a{
  align-self: center;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-self-center.png %}

{% codeblock lang:css %}
.item-a{
  align-self: stretch;
}
{% endcodeblock %}

{% img https://cdn.css-tricks.com/wp-content/uploads/2016/03/grid-align-self-stretch.png %}

为了让网格中的所有项都对齐，这个行为也可以通过设置网格容器中的`align-items`属性来实现。






[01]: https://css-tricks.com/snippets/css/complete-guide-grid/
[02]: https://github.com/zhaozhiming
[03]: http://abookapart.com/products/get-ready-for-css-grid-layout
[04]: https://css-tricks.com/snippets/css/a-guide-to-flexbox/
[05]: https://caniuse.com/#search=CSS%20grid%20layout
