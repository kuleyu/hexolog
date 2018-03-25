---
title: "javascript 变量提升相关"
comments: true
tags: ["javascript","变量提升"]
abbrlink: a33deebd
date: 2018-03-19 17:40:41
categories: ["学习笔记"]
---


### 什么是变量提升？
<b>变量提升</b>：把变量声明提升到`当前执行环境`的最顶端。按照js代码解析原则，js引擎在读取js代码时分两个步骤，第一个步骤是解释，第二个步骤是执行。所谓解释就是会先通篇扫描所有的Js代码，`然后把所有声明提升到顶端`；而执行就是操作一类的，依次执行解释完的代码。

### 变量提升大致可分为两类：
- `var 声明的变量的提升`。只将变量声明语句提升至当前执行环境的顶端，初始化语句（若有）则依然处于原位置不动。
- `function 声明的函数的提升`。将整个函数声明语句块提升至当前执行环境顶端，同时函数在声明时就已经将函数名初始化了。若有多个，则依次往下排，即先声明的位于最前。

<!-- more -->

#### 回顾
> - 一个变量的整个生命周期有三个阶段，<b>声明阶段</b>，<b>初始化阶段</b>，<b>赋值阶段</b>。只声明而未初始化的变量，其值默认为“undefined”。
```javascript
	var text;
	text+="你好";
	alert(text);  //"undefined你好"
```
> - 没经 var 声明，而直接初始化，会定义一个全局变量。
> - 注意，以表达式形式创建的函数没有提升作用。

### 实例分析
下面通过一些实例来了解一下吧。
- 例子1 关于下面代码：
```javascript
    (function(){
        console.log(typeof a); //"function"
        var a = 1;
        function a(){};
    })();
```
被javascript执行引擎解释后的形态，等同于这个：
```javascript
	(function() {
	    var a;
	    function a() {
	    }
	    console.log(typeof a); //"function"
	    a = 1;
	})();
```

- 例子2 关于下面代码：
```javascript
	(function(){
		console.log(typeof a); //function 
		var a = 1;
	    function a(){};
	    console.log(typeof a); //number
	})();
```
被javascript执行引擎解释后的形态，等同于这个：
```javascript
	(function(){ 
	    var a;
	    function a(){};
	    console.log(typeof a); //function
	    a = 1;
	    console.log(typeof a); //number
	})();
```

### 总结
不少文章中说的`“函数声明的提升优先于变量提升”其实并不准确`。函数声明的提升与变量提升都是根据声明的先后顺序依次排列的，只不过“变量声明只提升变量的声明语句；而函数声明提升的是整个语句块，关键一点是，函数声明时就已经对函数名初始化了”。
另外，以表达式形式创建的函数没有提升作用。


### 相关阅读：
[let深入理解---let存在变量提升吗？][01]








[01]: https://www.jianshu.com/p/0f49c88cf169
[02]: 
[03]: 
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