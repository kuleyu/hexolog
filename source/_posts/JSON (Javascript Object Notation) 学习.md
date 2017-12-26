---
title: JSON (Javascript Object Notation) 学习
date: '2017/06/10 21:32:17'
categories:
  - web前端技术
tags:
  - javascript
  - json
  - learning-notes
abbrlink: 23670
---


```
要点：
    1.理解json语法
    2.解析json
    3.序列化json
```

XML 与 JSON 都可以理解为在互联网上传输结构化数据的一种格式。XML 先于 JSON，但由于 ==XML 过于繁琐、冗长==，继而就又出现了 JSON。

JSON 是JavaScript 的一个严格的子集，利用了JavaScript中的一些模式来表示结构化数据。Crockford认为与XML相比，JSON是在JavaScript中读写结构化数据的更好的方式。因为==可以把JSON直接传给eval()，而且不必创建DOM对象==。另外，更重要的一个原因是，可以把JSON数据结构解析为有用的JavaScript对象。与XML数据结构要解析成DOM文档而且从中提取数据极为麻烦相比，JSON可以解析为JavaScript对象的优势极其明显。

关于JSON，最重要的是要理解它<font color="#26f3f1">是一种数据格式</font>，不是一种编程语言。虽然具有相同的语法形式，但JSON并不从属于JavaScript。而且，并不是只有JavaScript才使用JSON，毕竟JSON只是一种数据格式。很多编程语言都有针对JSON的解析器和序列化器。
<!-- more -->

## 语法

JSON的语法可以表示以下三种类型的值。
```
    1.简单值：使用与JavaScript相同的语法，可以在JSON中表示字符串、数值、布尔值和null。但JSON不支持JavaScript中的特殊值undefined。
    
    2.对象：对象作为一种复杂数据类型，表示的是一组有序的键值对儿。而每个键值对儿中的值可以是简单值，也可以是复杂数据类型的值。

    3.数值：数组也是一种复杂数据类型，表示一组有序的值的列表，可以通过数值索引来访问其中的值。数组的值也可以是任意类型——简单值、对象或数组。
```

### 简单值

简单值中，JSON字符串与JavaScript字符串的最大区别在于，JSON字符串必须使用==双引号==（单引号会导致语法错误）。

### 对象

JSON中的对象与JavaScript字面量稍微有一些不同。下面是一个JavaScript中的对象字面量：

```javascript
var person = { 
    name: "Nicholas",
    age: 29
};
```

这虽然是开发人员在JavaScript中创建对象字面量的标准方式，但JSON中的对象要求给属性加引号。实际上，在JavaScript中，前面的对象字面量完全可以写成下面这样：

```javascript
var object = { 
    "name": "Nicholas",
    "age": 29
};
```

JSON表示上述对象的方式如下：

```javascript
{ 
    "name": "Nicholas",
    "age": 29
}
```

与JavaScript的对象字面量相比，JSON对象有两个地方不一样。首先，没有声明变量（JSON中没有变量的概念）。其次，没有末尾的分号（因为这不是JavaScript语句，所以不需要分号）。再说一遍，对象的属性必须加双引号，这在JSON中是必需的。属性的值可以是简单值，也可以是复杂类型值，因此可以像下面这样在对象中嵌入对象：

```javascript
{ 
    "name": "Nicholas",
    "age": 29,
    "school": {
        "name": "Merrimack College",
        "location": "North Andover, MA"
    }
}
```

这个例子在顶级对象中嵌入了学校（"school"）信息。虽然有两个"name"属性，但由于它们分别属于不同的对象，因此这样完全没有问题。不过，同一个对象中绝对不应该出现两个同名属性。

与JavaScript不同，JSON中对象的属性名任何时候都必须加双引号。手工编写JSON时，忘了给对象属性名加双引号或者把双引号写成单引号都是常见的错误。

### 数组

JSON中的第二种复杂数据类型是数组。JSON数组采用的就是JavaScript中的数组字面量形式。例如，下面是JavaScript中的数组字面量：

```javascript
var values = [25, "hi", true];
```

在JSON中，可以采用同样的语法表示同一个数组：

```javascript
[25, "hi", true]
```

同样要注意，JSON数组也没有变量和分号。把数组和对象结合起来，可以构成更复杂的数据集合，例如：

```javascript
[
    {
         "title": "Professional JavaScript",
         "authors": [
             "Nicholas C. Zakas"
         ],
         edition: 3,
         year: 2011
    },
    {
         "title": "Professional JavaScript",
         "authors": [
              "Nicholas C. Zakas"
         ],
         edition: 2,
         year: 2009
    },
    {
         "title": "Professional Ajax",
         "authors": [
             "Nicholas C. Zakas",
             "Jeremy McPeak",
             "Joe Fawcett"
         ],
         edition: 2,
         year: 2008
    },
    {
         "title": "Professional Ajax",
         "authors": [
             "Nicholas C. Zakas",
             "Jeremy McPeak",
             "Joe Fawcett"
         ],
         edition: 1,
         year: 2007
    },
    {
         "title": "Professional JavaScript",
         "authors": [
              "Nicholas C. Zakas"
         ],
         edition: 1,
         year: 2006
    }
]
```

这个数组中包含一些表示图书的对象。每个对象都有几个属性，其中一个属性是"authors"，这个属性的值又是一个数组。对象和数组通常是JSON数据结构的最外层形式（当然，这不是强制规定的），利用它们能够创造出各种各样的数据结构。

## 解析与序列化

JSON之所以流行，拥有与JavaScript类似的语法并不是全部原因。更重要的一个原因是，可以把JSON数据结构解析为有用的JavaScript对象。与XML数据结构要解析成DOM文档而且从中提取数据极为麻烦相比，JSON可以解析为JavaScript对象的优势极其明显。就以上一节中包含一组图书的JSON数据结构为例，在解析为JavaScript对象后，只需要下面一行简单的代码就可以取得第三本书的书名：

```javascript
books[2].title
```

当然，这里是假设把解析后JSON数据结构得到的对象保存到了变量books中。再看看下面在DOM结构中查找数据的代码：

```javascript
doc.getElementsByTagName("book")[2].getAttribute("title")
```

看看这些多余的方法调用，就不难理解为什么JSON能得到JavaScript开发人员的热烈欢迎了。从此以后，JSON就成了Web服务开发中交换数据的事实标准。

### JSON 对象

早期的JSON解析器基本上就是使用JavaScript的eval()函数。由于JSON是JavaScript语法的子集，因此eval()函数可以解析、解释并返回JavaScript对象和数组。ECMAScript 5对解析JSON的行为进行规范，定义了全局对象JSON。支持这个对象的浏览器有IE 8+、Firefox 3.5+、Safari 4+、Chrome和Opera 10.5+。对于较早版本的浏览器，可以使用一个shim：https://github.com/douglascrockford/JSON-js 在旧版本的浏览器中，使用eval()对JSON数据结构求值存在风险，因为可能会执行一些恶意代码。对于不能原生支持JSON解析的浏览器，使用这个shim是最佳选择。

JSON对象有两个方法：==stringify()== 和 ==parse()==。在最简单的情况下，这两个方法分别用于把JavaScript对象序列化为JSON字符串和把JSON字符串解析为原生JavaScript值。例如：

```javascript
var book = { 
                title: "Professional JavaScript",
                authors: [
                   "Nicholas C. Zakas"
                ],
                edition: 3,
                year: 2011
           };

var jsonText = JSON.stringify(book);
```

JSONStringifyExample01.htm

这个例子使用JSON.stringify()把一个JavaScript对象序列化为一个JSON字符串，然后将它保存在变量jsonText中。默认情况下，JSON.stringify()输出的JSON字符串不包含任何空格字符或缩进，因此保存在jsonText中的字符串如下所示：
这个例子使用JSON.stringify()把一个JavaScript对象序列化为一个JSON字符串，然后将它保存在变量jsonText中。默认情况下，JSON.stringify()输出的JSON字符串不包含任何空格字符或缩进，因此保存在jsonText中的字符串如下所示：

```javascript
{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":3,
"year":2011}
```

++在序列化JavaScript对象时，所有函数及原型成员都会被有意忽略，不体现在结果中。此外，值为undefined的任何属性也都会被跳过。++ 结果中最终都是值为有效JSON数据类型的实例属性。

将JSON字符串直接传递给JSON.parse()就可以得到相应的JavaScript值。例如，使用下列代码就可以创建与book类似的对象：

```
var bookCopy = JSON.parse(jsonText);
```

注意，虽然book与bookCopy具有相同的属性，但它们是两个独立的、没有任何关系的对象。

如果传给JSON.parse()的字符串不是有效的JSON，该方法会抛出错误。

### 序列化选项

实际上，JSON.stringify()除了要序列化的JavaScript对象外，还可以接收另外两个参数，这两个参数用于指定以不同的方式序列化JavaScript对象。第一个参数是个过滤器，可以是一个数组，也可以是一个函数；第二个参数是一个选项，表示是否在JSON字符串中保留缩进。单独或组合使用这两个参数，可以更全面深入地控制JSON的序列化。

1. 过滤结果

如果过滤器参数是数组，那么JSON.stringify()的结果中将只包含数组中列出的属性。来看下面的例子。

```javascript
var book = { 
               "title": "Professional JavaScript",
                "authors": [
                   "Nicholas C. Zakas"
                ],
                edition: 3,
                year: 2011
           };

var jsonText = JSON.stringify(book, ["title", "edition"]);
```

JSONStringifyExample01.htm

JSON.stringify()的第二个参数是一个数组，其中包含两个字符串："title"和"edition"。这两个属性与将要序列化的对象中的属性是对应的，因此在返回的结果字符串中，就只会包含这两个属性：

```javascript
{"title":"Professional JavaScript","edition":3}
```

如果第二个参数是函数，行为会稍有不同。传入的函数接收两个参数，属性（键）名和属性值。根据属性（键）名可以知道应该如何处理要序列化的对象中的属性。属性名只能是字符串，而在值并非键值对儿结构的值时，键名可以是空字符串。

为了改变序列化对象的结果，函数返回的值就是相应键的值。++不过要注意，如果函数返回了undefined，那么相应的属性会被忽略。++ 还是看一个例子吧。

```javascript
var book = { 
                "title": "Professional JavaScript",
                "authors": [
                    "Nicholas C. Zakas"
                ],
                edition: 3,
                year: 2011
          };

var jsonText = JSON.stringify(book, function(key, value){
    switch(key){
        case "authors":
            return value.join(",")

        case "year":
            return 5000;

        case "edition":
            return undefined;

        default:
            return value;
    }
});
```

JSONStringifyExample02.htm

这里，函数过滤器根据传入的键来决定结果。如果键为"authors"，就将数组连接为一个字符串；如果键为"year"，则将其值设置为5000；如果键为"edition"，通过返回undefined删除该属性。最后，一定要提供default项，此时返回传入的值，以便其他值都能正常出现在结果中。实际上，第一次调用这个函数过滤器，传入的键是一个空字符串，而值就是book对象。序列化后的JSON字符串如下所示：

```javascript
{"title":"Professional JavaScript","authors":"Nicholas C. Zakas","year":5000}
```

要序列化的对象中的每一个对象都要经过过滤器，因此数组中的每个带有这些属性的对象经过过滤之后，每个对象都只会包含"title"、"authors"和"year"属性。

Firefox 3.5和3.6对JSON.stringify()的实现有一个bug，在将函数作为该方法的第二个参数时这个bug就会出现：过滤函数返回undefined意味着要跳过某个属性，而返回其他任何值都会在结果中包含相应的属性。Firefox 4修复了这个bug。

2. 字符串缩进

JSON.stringify()方法的第三个参数用于控制结果中的缩进和空白符。如果这个参数是一个数值，那它表示的是每个级别缩进的空格数。例如，要在每个级别缩进4个空格，可以这样写代码：

```javascript
var book = { 
                "title": "Professional JavaScript",
                "authors": [
                    "Nicholas C. Zakas"
                ],
                edition: 3,
                year: 2011
           };

var jsonText = JSON.stringify(book, null, 4);
```

JSONStringifyExample03.htm

保存在jsonText中的字符串如下所示：

```javascript
{ 
    "title": "Professional JavaScript",
    "authors": [
        "Nicholas C. Zakas"
    ],
    "edition": 3,
    "year": 2011
}
```

不知道读者注意到没有，JSON.stringify()也在结果字符串中插入了换行符以提高可读性。++只要传入有效的控制缩进的参数值，结果字符串就会包含换行符。（只缩进而不换行意义不大。）最大缩进空格数为10，所有大于10的值都会自动转换为10。++

如果缩进参数是一个字符串而非数值，则这个字符串将在JSON字符串中被用作缩进字符（不再使用空格）。在使用字符串的情况下，可以将缩进字符设置为制表符，或者两个短划线之类的任意字符。

```javascript
var jsonText = JSON.stringify(book, null, " — -");
```

这样，jsonText中的字符串将变成如下所示：

```javascript
{
  "title": "Professional JavaScript",
  "authors": [
    "Nicholas C. Zakas"
  ],
  "edition": 3,
  "year": 2011
}
```

缩进字符串最长不能超过10个字符长。如果字符串长度超过了10个，结果中将只出现前10个字符。

3. toJSON()方法

有时候，JSON.stringify()还是不能满足对某些对象进行自定义序列化的需求。在这些情况下，可以通过对象上调用toJSON()方法，返回其自身的JSON数据格式。原生Date对象有一个toJSON()方法，能够将JavaScript的Date对象自动转换成ISO 8601日期字符串（与在Date对象上调用toISOString()的结果完全一样）。

可以为任何对象添加toJSON()方法，比如：

```javascript
var book = {
            "title": "Professional JavaScript",
             "authors": [
                 "Nicholas C. Zakas"
            ],
            edition: 3,
            year: 2011,
             toJSON: function(){
                      return this.title;
                 }
           };

var jsonText = JSON.stringify(book);
```

JSONStringifyExample05.htm

以上代码在book对象上定义了一个toJSON()方法，该方法返回图书的书名。与Date对象类似，这个对象也将被序列化为一个简单的字符串而非对象。可以让toJSON()方法返回任何序列化的值，它都能正常工作。也可以让这个方法返回undefined，此时如果包含它的对象嵌入在另一个对象中，会导致该对象的值变成null，而如果包含它的对象是顶级对象，结果就是undefined。

toJSON()可以作为函数过滤器的补充，因此理解序列化的内部顺序十分重要。假设把一个对象传入JSON.stringify()，序列化该对象的顺序如下。
1. 如果存在toJSON()方法而且能通过它取得有效的值，则调用该方法。否则，按默认顺序执行序列化。
2. 如果提供了第二个参数，应用这个函数过滤器。传入函数过滤器的值是第1步返回的值。
3. 对第2步返回的每个值进行相应的序列化。
4. 如果提供了第三个参数，执行相应的格式化。

无论是考虑定义toJSON()方法，还是考虑使用函数过滤器，亦或需要同时使用两者，理解这个顺序都是至关重要的。

### 解析选项

JSON.parse()方法也可以接收另一个参数，该参数是一个函数，将在每个键值对儿上调用。为了区别JSON.stringify()接收的替换（过滤）函数（replacer），这个函数被称为还原函数（reviver），但实际上这两个函数的签名是相同的——它们都接收两个参数，一个键和一个值，而且都需要返回一个值。

如果还原函数返回undefined，则表示要从结果中删除相应的键；如果返回其他值，则将该值插入到结果中。在将日期字符串转换为Date对象时，经常要用到还原函数。例如：

```javascript
var book = { 
              "title": "Professional JavaScript",
               "authors": [
                   "Nicholas C. Zakas"
                ],
                edition: 3,
                year: 2011,
                releaseDate: new Date(2011, 11, 1)
           };

var jsonText = JSON.stringify(book);

var bookCopy = JSON.parse(jsonText, function(key, value){
    if (key == "releaseDate"){
        return new Date(value);
    } else {
        return value;
    }
});

alert(bookCopy.releaseDate.getFullYear());
```

JSONParseExample02.htm

以上代码先是为book对象新增了一个releaseDate属性，该属性保存着一个Date对象。这个对象在经过序列化之后变成了有效的JSON字符串，然后经过解析又在bookCopy中还原为一个Date对象。还原函数在遇到"releaseDate"键时，会基于相应的值创建一个新的Date对象。结果就是bookCopy.releaseDate属性中会保存一个Date对象。正因为如此，才能基于这个对象调用getFullYear()方法。

### 小结

JSON是一个轻量级的数据格式，可以简化表示复杂数据结构的工作量。JSON使用JavaScript语法的子集表示对象、数组、字符串、数值、布尔值和null。即使XML也能表示同样复杂的数据结果，但JSON没有那么烦琐，而且在JavaScript中使用更便利。

ECMAScript 5定义了一个原生的JSON对象，可以用来将对象序列化为JSON字符串或者将JSON数据解析为JavaScript对象。JSON.stringify()和JSON.parse()方法分别用来实现上述两项功能。这两个方法都有一些选项，通过它们可以改变过滤的方式，或者改变序列化的过程。

原生的JSON对象也得到了很多浏览器的支持，比如IE8+、Firefox 3.5+、Safari 4+、Opera 10.5和Chrome。