---
title: Js变量提升、临时死区、作用域、立即执行函数
comment: true
draft: false
categories:
  - ''
  - ''
tags:
  - ''
  - ''
  - ''
author: '<a href="http://captaininphw.xyz/" rel="noopener" target="_blank">戴江涛</a>'
weight: 10
contentCopyright: >-
  <a
  href="http://captaininphw.xyz/2018/03/16/CSS-%E5%B8%B8%E8%A7%81%E5%B8%83%E5%B1%80/"
  rel="noopener" target="_blank">See origin</a>
hiddenFromHomePage: true
abbrlink: 56be0507
date: 2018-03-27 18:37:02
lastmod: 2018-03-27 18:37:02
---

提到 JS 中声明变量的方式，必然提及var、let、const、function 四个关键词，其中 var、function 声明的变量会发生变量提升。

### var
var 是初学者常用的声明变量的方式，简单的，声明任何数据都可以用 var :

```js
var num = 1；// declare a number
var str = ''; // declare a string
var bool = true； // declare a boolean
var arr = []; // declare a array
var obj = {}; // declare a object
var fn = function (){}; // declare a function
```

以上声明变量的方式避免不了会发生变量提升，什么意思呢？以正常的思维来看，如果一个变量还未声明，那么该变量就不可用。但是 JS 中使用 var 声明的变量会发生提升，即 JS 引擎在解释语句时，遇到 var 声明的变量会把该变量放置于当前作用域的最前面，同时初始化为 undefined，且函数的提升在变量之前。举例：

```js
getA();
var a = 1;
function getA(){
    console.log(a);
}
// print undefined
```

为什么会打印出 undefined 呢？因为 a 发生了变量提升，且在 a 被赋值之前就使用了 a，此时 a 的值为 undefined，该段代码执行时的实际情形如下：

```js
function getA(){
    console.log(a);
}
var a; // initialize 'a' to 'undefined'
getA(); // print undefined
a = 1; // then a is assigned to 1
```

再来看下面的一种情况：

```js
var a = 1;
function getA(){
    console.log(a);
    var a = 2;
    console.log(a);
}
getA(); // first print out undefined, then print out 2
```

那么上面的代码执行时的实际情形如下：

```js
function getA(){
    var a; // initialize a to undefined
    console.log(a); // then print out undefined
    a = 2; // a is assigned to 2
    console.log(a); // then print out 2
}
var a; // this 'a' is global variable
a = 1; // global a is assigned to 1
getA();
```

上段代码中第一次打印出的为什么不是 1 呢？如果不清楚变量提升以及作用域那么很容易犯这种低级错误。上段代码在执行时， getA 函数中首先将 var a 提升至 当前作用域 的最前面，即 getA 函数中的最前面。代码在执行时如何取值呢？当然是先看自己当前作用域有没有该值，如果有，就用当前作用域的值，如果没有，则顺着作用域链向上找，直到找到该变量为止。如何让上段代码输出 1 以及 2 该怎么办呢？很简单，去掉 getA 函数中的 var 即可。

```js
var a = 1;
function getA(){
    console.log(a); // print out global a's value: 1
    a = 2; // global a is assigned to 2
    console.log(a); // print out global a's value: 2
}
getA();
console.log(a); // print out global a's value: 2
```

因为 getA 函数中没有 a，则顺着作用域链向上找，发现函数外有一个 a 变量，则打印出该变量中存储的值 1 ，此时打印出的 a 是函数外部的 a，再执行 a = 2 时，全局的 a 被赋值为 2。

那么问题来了：声明变量不加关键字一定会声明为全局变量吗？

答：如果函数外部没有同名的全局变量的话，那么就会生成全局变量。

举例一：

```js
function getA(){
    console.log(a); // error: a is not defined
    a = 1; // a is declared to global variable
}
getA();
console.log(a); // print out 1
```

第一次打印出 a 时，函数内部没有声明的变量 a，顺着作用域链找也没有 a，就会抛出错误，第二次打印出 a 的时候，函数 getA 中声明了全局变量 a，会打印出 1。当然上述代码抛出错误之后，后面的语句不会执行，可以注释掉 getA 函数中的第一条语句再运行。

举例二：

```js
a = 1;
function getA(){
    console.log(a); // print 1
    a = 2; // global a is assigned to 2
}
getA();
console.log(a); // print out 2
```

上段代码中，getA 函数外部有一个全局变量 a，getA 中要声明与全局变量同名的变量时没有加变量关键字，因此 a = 2 的作用为将全局变量 a 赋值为 2。因此可以看出，声明变量时一定要加上变量关键字，否则会产生预料之外的错误。

如果一个变量没有定义就可以使用，是非常令人困惑的。针对这种情况，ES6 推出了声明变量的新关键字 let 以及 const 。

### let
let 关键字声明的变量是不会发生变量提升的，将之前的代码中的 var 改为 let 看看结果：

```js
getA();
let a = 1;
function getA(){
    console.log(a); // error: a is not defined
}
```

上面的代码执行实际情况为：

```js
function getA(){
    console.log(a);
}
getA(); // error: a is not defined
let a; // initialize a to undefined
a = 1; // then a is assigned to 1
```

在函数内部用 let 声明变量是一样的。

```js
let a = 1;
function getA(){
    console.log(a); // error: a is not defined
    let a = 2;
    console.log(a); // print out 2
}
getA();
```

上段代码也很好理解，关键是要理解作用域。

那么如果上段代码中想要先打印出函数外 a 的值，再声明函数内部的私有变量 a 可以吗？

答案是不可以，因为 let 解决了变量提升这个问题时，同时带来了另一个问题，那就是临时死区（Temporal Dead Zone， TDZ）。通俗的理解就是，若当前作用域中使用 let 关键字定义了与作用域外部同名的变量，那么在当前作用域内，定义同名变量之前，都不可以使用该变量，即使你的本意是想先使用外部同名变量，再定义内部同名变量。

```js
let a = 1;
function getA(){
	// 在下面这句语句执行之前使用 a 都会报错，即使你想使用的是外面的 a
    let a = 2;
    console.log(a); // print out 2
}
getA();
```

### const

const 与 let 类似，都没有变量提升，都存在临时死区。不同的是，const 声明的变量，只能在声明的同时初始化，之后是不允许赋值的（Object 类型数据除外），否则会报错。若没有在声明的同时初始化，也会报错。

```js
const a = 1;
a = 2; // error: Assignment to constant variable

const b; // error: Missing initializer in const declaration
b = 1;
```

那么看下面的代码：

```js
const obj = {
    name: 'daijt',
    age: 18
};
obj.nickName = 'captain';
console.log(obj); // {name: 'daijt', age: 18, nickName: 'captain'}
```

为什么 const 声明的变量又可以修改其中的数据了呢？因为 obj 是个复杂数据，不是简单数据。conts 声明变量的本质是变量中的数据紧致修改，为什么复杂数据可以更改呢？因为 const 声明的变量中存储的是复杂对象的引用地址，而不是真真的数据，仅仅是数据的地址。因此在使用 const 声明了变量来引用复杂数据之后，还是可以修改该复杂数据的值。复杂数据有哪些呢？array Object、object Object、function Object 等。不建议使用 const 声明复杂数据，因为如果稍加不注意，就会更改了不想被改变的复杂数据的值。建议使用 const 声明简单数据，同时变量名大写。为什么简单数据的更改就能检测出来呢？因为简单数据是直接存储在栈内存中的，而不是像复杂对象，栈内存中存储的是堆内存中的引用地址。

### 作用域

说起作用域，ES6 新引入了一个块级作用域，之前 ES5 只有全局作用域与函数作用域。

在 ES6 之前，如果想要定义一个局部变量/私有变量该怎么办呢？答案是利用函数作用域。如果不想定义具名函数，浪费命名空间的话，可以使用立即执行函数（Immediately Invoked Function Expression， IIFE），如何定义立即执行函数？以下可作参考：

```js
( function(){ code } ).call(); // can return value

( function(){ code } .call()); // can return value

( function(){ code } )(); // can return value

! function(){ code } ();

~ function(){ code } ();

+ function(){ code } ();

- function(){ code } ();
```

以上几种都是 IIFE，值得注意的是前三种是可以有返回值的。数据可以通过括号传递。例如：

```js
let a = (function (num1, num2){
    return num1+num2;
})(1,2);
console.log(a); // print out 3
```

ES6 中引入了块级作用域，那么声明私有变量/局部变量不用再利用函数作用域了，直接使用块级作用域 {} 即可，值得注意的是在花括号内使用 var 是没有用的，因为 ES5 没有块级作用域的概念。

```js
// in ES6
let a = 1;
{
    let a = 2;
    console.log(a); // print 2
}
console.log(a); // print 1
// in ES5
var a = 1;
{
    var a = 2;
    console.log(a); // print 2
}
console.log(a); // print 2
```

那么由作用域可以引入一个经典问题，问以下代码的执行完结果是什么？

```js
for(var i = 0; i < 5; i++){
    setTimeout(function (){
        console.log(i);
    },1000)
}
// what will the console print out?
```

很多人都知道会在 1s 之后打印出 5 个 5 。为什么呢？可以结合变量提升和作用域进行分析。由于 var 没有块级作用域，因此 var i 会声明 i 为全局变量，以上代码执行时情况如下：

```js
{
    var i = 0;
    setTimeout(function (){
        console.log(i);
    },1000)
}
...
...
...
{
    var i = 4;
    setTimeout(function (){
        console.log(i);
    },1000)
}
```

进一步拆分：

```js
var i;
i = 0;
i = 1;
i = 2;
i = 3;
i = 4;
i = 5;
console.log(i);
console.log(i);
console.log(i);
console.log(i);
console.log(i);
```
那么 i = 5 是怎么来的呢？因为 i 为全局变量，在不满足循环条件的时候 i === 5，所以在 1s 之后打印出 5 个 5。那么如何打印出 0、1、2、3、4 呢？最简单的方法，将 var 改为 let：

```js
for(let i = 0; i < 5; i++){
    setTimeout(function (){
        console.log(i);
    },1000)
}
```

将改为 let 的代码进行拆分，如下：

```js
{
    let i = 0;
    setTimeout(function (){
        console.log(i);
    },1000);
}
...
...
...
{
    let i = 4;
    setTimeout(function (){
        console.log(i);
    },1000);
}
```

由于使用了 let，因此花括号为块级作用域，内部的 i 为局部变量，延时函数在执行时，会优先在当前作用域访问 i，因此会打印出 0、1、2、3、4 。 此外还有其他方法，那就是利用立即执行函数:

```js
for(var i = 0; i < 5; i++){
    setTimeout(function (){
        console.log(i);
    }.call(),1000)
}
// print out 0、1、2、3、4
```

这其中要涉及到事件队列，setTimeout 将第一个参数推入 Event queue 时，发现是个立即执行函数，则立即执行，打印出当前的 i 值。或者还可以改写如下：

```js
for(var i = 0; i < 5; i++){
    setTimeout(console.log(i),1000);
}
// print out 0、1、2、3、4
```