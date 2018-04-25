---
title: "css 浮动与清除浮动"
comments: true
abbrlink: 3a176a24
date: 2018-03-19 17:41:48
categories: ["学习笔记"]
tags: ["css","float"]
---




最好用的---:after伪元素法：
```css
	.clearfix:after {
		content: "";
		display: block;
		height: 0;
		/*visibility: hidden;*/
		clear: both;
	}
	.clearfix {
		*zoom: 1;  /*由于IE6-7不支持:after，使用 zoom:1触发 hasLayout*/
	}
```



<p data-height="300" data-theme-id="30438" data-slug-hash="QmdrBm" data-default-tab="html,result" data-user="kuleyu" data-embed-version="2" data-pen-title="QmdrBm" class="codepen">See the Pen <a href="https://codepen.io/kuleyu/pen/QmdrBm/">QmdrBm</a> by kuleyu (<a href="https://codepen.io/kuleyu">@kuleyu</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

------------
**参考**
1. [那些年我们一起清除过的浮动][01]


[01]: http://www.iyunlu.com/view/css-xhtml/55.html
[02]: https://juejin.im/entry/584f9d3a61ff4b006cd9dff8
[03]: http://www.cssmojo.com/clearfix_block-formatting-context_and_hasLayout/
[04]: 
[05]: 
[06]: 
[07]: 
[08]: 
[09]: 
[10]: 
[11]: 
[12]: 
[13]: 
[14]: 
[15]: 
[16]: 
[17]: 
[18]: 
[19]: 
[20]: 