---
title: "正确认识 JavaScript 中的 this"
date: 2018-01-20 19:21:09
abbrlink: 3a977c25
comment: true
categories: ["学习笔记"]
tags: ["JavaScript"]
weight: 10
---

> 原创，转载请注明出处。

## `this` 的定义

要正确了解 `this` ，还得先从其定义入手：

  1. `this` 是函数内部的两个特殊对象之一（另一个为 `arguments`）；
  2. `this` 引用的是其所属函数被执行时的环境对象（说白了就是**this所属函数的父级对象**）
  3. 由于在调用函数之前，其父级对象是不确定的，因此 `this` 也是不确定的，可能会在代码执行过程中引用不同的对象。

> 似乎没有 `所属函数`和`父级对象` 这两词，只不过我觉得用这两词来描述可能会更容易正确的理解一些。因为《JavaScript 高级程序设计》（中文版）中，后两点确实不太好理解，一个 `this对象`，一个 `this值`，以至于我之前一直将其错误地理解成所属函数的引用。

<!-- more -->

## 案列分析

下面再来分析一下，当函数在不同作用域下被调用时，其 `this` 对象所引用的对象。

### 一般情况

```javascript
var color = 'red';
var o ={color:'blue'};

function sayColor() {
  console.log(this.color)
}

function fN() {
  var color = 'pink';
  console.log(this.color)
}

console.log(window.color);        // "red"
sayColor();                       // "red"  （此处，sayColor是在全局作用域下调用的。sayColor()与window.sayColor()是一样的。）
fN();                             // "red"   (看你有没有正真理解第二点😂)
```

### 引入`.apply()`和`.call()`的情况

引入`.apply()`和`.call()`可以扩充函数赖以运行的作用域。

```javascript
var color = 'red';
var o ={color:'blue'};

function sayColor() {
  console.log(this.color)
}

sayColor.call(this);              // "red"
sayColor.call(window);            // "red"
sayColor.call(null);              // "red"  (此时sayColor的this指向window)
sayColor.call(o);                 // "blue"  (此时sayColor的this指向对象o)

sayColor.apply(this);              // "red"
sayColor.apply(window);            // "red"
sayColor.apply(null);              // "red" (此时sayColor的this指向window)
sayColor.apply(o);                 // "blue"

o.sayColor();                     // TypeError: o.sayColor is not a function
```

### 引入`.bind()`的情况

```javascript
var color = 'red';
var o ={color:'blue'};

function sayColor() {
  console.log(this.color)
}

sayColor.bind(o);                        // 函数调用.bind()方法只是创建一个该函数的实例，并不会立即执行
sayColor.bind(o)();                      // "blue"  （此时，sayColor函数的this指向对象o）

var objectSayColor = sayColor.bind(o);
objectSayColor();                        // "blue"   （此时，sayColor函数的this指向对象o）
```

###  bind与apply和call的区别

> `.apply()`和`.call()`、`.bind()` 是函数的三个固有方法。前两者非继承而来的，是用来调用函数的；而后者是ES5中定义的，用来创建函数实例的。

  - `.apply()`和`.call()`都是改变`该方法所属函数`中的`this`,并立即执行这个函数；
  - `.bind()`可以改变`该方法所属函数`中的`this`，不过它只是创建一个该函数的实例，并不会立即执行，也因此你可以让对应的函数想什么时候调就什么时候调用，并且可以将参数在执行的时候添加。

鉴于以上区别，我们可以根据实际情况来选择使用。
