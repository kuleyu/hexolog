---
title: Js作用域问题一步一步透彻理解
comment: true
draft: false
categories:
  - JavaScript
  - 学习笔记
tags:
  - js
  - 作用域
weight: 10
abbrlink: ae13e827
date: 2018-04-01 10:09:34
lastmod: 2018-04-01 10:09:34
---

## **黄金守则第一条**

js没有块级作用域（你可以自己闭包或其他方法实现），只有函数级作用域，函数外面的变量函数里面可以找到，函数里面的面找不到。

```javascript
var a = 10;
function aaa() {
	alert(a);
}
function bbb() {
	var a = 20;
	aaa();
}
bbb();      // 10
```

<!-- more -->

## **黄金守则第二条**

变量的查找是就近原则，去寻找var定义的变量，当就近没有找到的时候就去查找外层。

```javascript

function aaa() {    
	alert(a);
	var a=b=10;  //相当于 var a = 10; b=10; 此时 b 为全局变量，a 函数 aaa 的只是局部变量
}
aaa();	      // undefined ----- var变量声明提前（仅仅将声明提前，初始化仍在远处进行）
alert(b);     // 10
alert(a);     // console面板报错 “ReferenceError: a is not defined”    
```

## **黄金守则第三条**

当参数跟局部变量重名时，优先级是等同的。

```javascript
var a = 10;
function aaa(a) {
	alert(a);
	var a = 20;
}
aaa(a);      // 10
aaa();       // undefined
```

## **黄金守则第四条**

参数的传递都是`按值传递`。基本类型的这个值指基本类型值，引用类型的这个值指引用类型的指针。

### **变量的情况**

```javascript
var a = 5;
var b = a;
b +=3;
alert(a);//5
var a = [1,2,3];
var b=a;
b.push(4);
alert(a);//[1,2,3,4];
```

引用类型变量重新赋值后，会不一样：
```javascript
var a = [1,2,3];
var b=a;
b = [1,2,3]    // 此处，b被重新赋值了，也就不再指向a了。
b.push(4);
alert(a);//[1,2,3];
```

### **参数的情况**

参数与变量的作用域是相似的：

```javascript
var a = 10;
function aaa(a) {
	a += 3;
	alert(a);
}
aaa(a);    // 13
alert(a);  // 10
```


```javascript
var a = [1,2,3];
function aaa(a) {
	a.push(4);
}
aaa(a);
alert(a);  // 1,2,3,4
```

引用类型参数重新赋值后，会不一样：
```javascript
var a = [1,2,3];
function aaa(a) {
	a = [1,2,3];
	a.push(4);
}
aaa(a);
alert(a);  // 1,2,3
```


## **最后一个比较怪的**

```javascript
var arry = [];
arry[0] = 'a';
arry[1] = 'b';
arry.foo = 'c';
arry['foo'] = 'd';            //不报错
console.log(typeof arry.foo);    //  step2:   string
console.log(typeof arry['foo']);  //  step3:   string
console.log(arry.foo);           //   step4:   d
console.log(arry['foo']);        //   step5:   d
console.log(arry.length);  		// step6:   2
alert(arry);               		// step1:   a,b
arry[foo] = 'e';             //报错   ReferenceError: foo is not defined
```

- 因为：
	1. 数组对象只有一个属性，那就是 `lenght` (同时，其他普通对象都没有 `length` 属性)；
	2. 数组也是对象；
	3. 对象属性的访问方式有两种，`.` 与 `[]` (`[]` 内字符串属性要加 `""`)；
	4. 个人推断： 数组作为对象的一种，可以对其使用普通对象的所有方法与属性，但不属于数组对象的属性和方法并不会对数组产生实质影响。


## **参考**
- [js作用域问题一步步透彻理解](https://www.cnblogs.com/skylar/p/3986087.html#comment_tip)---[skylar艺璇](http://zhangmengxue.com)
- javascript 高级程序设计
- [JS 对象属性访问的2种方式和用途](https://blog.csdn.net/shuren1991/article/details/67639250)
	> 内容：判断一个字符串中出现次数最多的字符，统计这个次数