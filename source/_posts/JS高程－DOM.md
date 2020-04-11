---
title: JS高程－DOM
categories:
  - JavaScript
  - 学习笔记
tags:
  - dom
comment: true
weight: 10
abbrlink: ab8e0436
date: 2017-08-21 00:00:00
---

> DOM（文档对象模型）是针对HTML和XML文档的一个API（应用程序编程接口），描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。


> DOM１级规范成为W3C的推荐标准，为基本的文档结构及查询提供了接口。本章主要讨论与浏览器中的HTML页面相关的DOM1级的特性和应用，以及JavaScript对DOM1级的实现。IE、Firefox、Safari、Chrome和Opera都非常完善地实现了DOM。

### 节点层次

DOM可以将任何HTML或XML文档描绘成一个由多层节点构成的结构。节点分为几种不同的类型，每种类型分别表示文档中不同的信息及（或）标记。每个节点都拥有各自的特点、数据和方法，另外也与其他节点存在某种关系。节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点为根节点的树形结构。以下面的HTML为例：

```html
<html>
    <head>
        <title>Sample Page</title>
    </head>
    <body>
        <p>Hello World!</p>
    </body>
</html>
```
对应树形结构如下：
```
Document   // 根节点
|--Element   // html   根节点的子节点称为"文档节点"
|  |--Element    // head
|     |--Element    // title
|        |--Text    // Sample Page
|  |--Element    // body      
|     |--Element    // p
|        |--Text    // Hello World!
```

每一段标记都可以通过树中的一个节点来表示：HTML元素通过元素节点表示，特性（attribute）通过特性节点表示，文档类型通过文档类型节点表示，而注释则通过注释节点表示。总共有12种节点类型，这些类型都继承自一个基类型，即 `Node` 类型。

#### `Node` 类型

DOM1级定义了一个Node接口，这个Node接口在JavaScript中是作为Node类型实现的；除了IE之外，在其他所有浏览器中都可以访问到这个类型。JavaScript中的所有节点类型都继承自Node类型，因此所有节点类型都共享着相同的基本属性和方法。

每个节点都有一个 `nodeType` 属性，用于表明节点的类型。节点类型由在Node类型中定义的下列12个数值常量来表示，任何节点类型必居其一：

- Node.ELEMENT_NODE(1)；
- Node.ATTRIBUTE_NODE(2)；
- Node.TEXT_NODE(3)；
- Node.CDATA_SECTION_NODE(4)；
- Node.ENTITY_REFERENCE_NODE(5)；
- Node.ENTITY_NODE(6)；
- Node.PROCESSING_INSTRUCTION_NODE(7)；
- Node.COMMENT_NODE(8)；
- Node.DOCUMENT_NODE(9)；
- Node.DOCUMENT_TYPE_NODE(10)；
- Node.DOCUMENT_FRAGMENT_NODE(11)；
- Node.NOTATION_NODE(12)。

通过比较上面这些常量，可以很容易地确定节点的类型，例如：

```javascript
if (someNode.nodeType == Node.ELEMENT_NODE){   //在IE中无效
    alert("Node is an element.");
}

if (someNode.nodeType == 1){    //适用于所有浏览器
    alert("Node is an element.");
}
```

##### **`nodeName` 与 `NodeValue` 属性**

 在使用这两个值以前，最好是像下面这样先检测一下节点的类型。
 
```javascript
if (someNode.nodeType == 1){
    var nameN = someNode.nodeName;    //nodeName的值是元素的标签名
    var valueN = someNode.nodeValue;  //元素的nodeValue的值则始终为null
}
```

##### **节点关系**

> `.childNodes`，`.parentNode`，`.previousSibling` 和 `.nextSibling`，`.firstChild` 和 `.lastChild`，`.hasChildNodes()`，`.ownerDocument`

每个节点都有一个 `childNodes` 属性，其中保存着一个NodeList对象。NodeList是一种类数组对象，用于保存一组有序的节点，可以通过位置来访问这些节点。请注意，虽然可以通过方括号语法来访问NodeList的值，而且这个对象也有length属性，但它并不是Array的实例。NodeList对象的独特之处在于，它实际上是基于DOM结构动态执行查询的结果，因此DOM结构的变化能够自动反映在NodeList对象中。NodeList就好像是有生命、有呼吸的对象，而不是在我们第一次访问它们的某个瞬间拍摄下来的一张快照。

下面的例子展示了如何访问保存在NodeList中的节点——可以通过方括号，也可以使用item()方法。

```javascript
var firstChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);
var count = someNode.childNodes.length;
```

本书前面介绍过，对arguments对象使用Array.prototype.slice()方法可以将其转换为数组。而采用同样的方法，也可以将NodeList对象转换为数组。来看下面的例子：

```javascript
//在IE8及之前版本中无效
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);
```

除IE8及更早版本之外，这行代码能在任何浏览器中运行。由于IE8及更早版本将NodeList实现为一个COM对象，而我们不能像使用JScript对象那样使用这种对象，因此上面的代码会导致错误。要想在IE中将NodeList转换为数组，必须手动枚举所有成员。下列代码在所有浏览器中都可以运行：

```javascript
function convertToArray(nodes){
    var array = null;
    try {
        array = Array.prototype.slice.call(nodes, 0); //针对非IE浏览器
    } catch (ex) {
        array = new Array();
        for (var i=0, len=nodes.length; i < len; i++){
            array.push(nodes[i]);
        }
    }

    return array;
}
```

每个节点都有一个 `parentNode` 属性，该属性指向文档树中的父节点。包含在childNodes列表中的所有节点都具有相同的父节点，因此它们的parentNode属性都指向同一个节点。此外，包含在childNodes列表中的每个节点相互之间都是同胞节点。通过使用列表中每个节点的 `previousSibling` 和 `nextSibling` 属性，可以访问同一列表中的其他节点。列表中第一个节点的previousSibling属性值为null，而列表中最后一个节点的nextSibling属性的值同样也为null。

父节点与其第一个和最后一个子节点之间也存在特殊关系。父节点的 `firstChild` 和 `lastChild` 属性分别指向其childNodes列表中的第一个和最后一个节点。其中，someNode.firstChild的值始终等于someNode.childNodes[0]，而someNode.lastChild的值始终等于someNode.childNodes [someNode.childNodes.length-1]。在只有一个子节点的情况下，firstChild和lastChild指向同一个节点。如果没有子节点，那么firstChild和lastChild的值均为null

在反映这些关系的所有属性当中，childNodes属性与其他属性相比更方便一些，因为只须使用简单的关系指针，就可以通过它访问文档树中的任何节点。另外，`hasChildNodes()` 也是一个非常有用的方法，这个方法在节点包含一或多个子节点的情况下返回true；应该说，这是比查询childNodes列表的length属性更简单的方法。

所有节点都有的最后一个属性是 `ownerDocument`，该属性指向表示整个文档的`文档节点`。这种关系表示的是任何节点都属于它所在的文档，任何节点都不能同时存在于两个或更多个文档中。通过这个属性，我们可以不必在节点层次中通过层层回溯到达顶端，而是可以直接访问文档节点。


##### **操作节点**

> `.appendChild()`，`.insertBefore()`，`.replaceChild()`，`.removeChild()`；`.cloneNode()`，`.normalize()`。

因为关系指针都是只读的，所以DOM提供了一些操作节点的方法。其中，最常用的方法是 `appendChild()`，用于向childNodes列表的末尾添加一个节点。添加节点后，childNodes的新增节点、父节点及以前的最后一个子节点的关系指针都会相应地得到更新。更新完成后，appendChild()返回新增的节点。来看下面的例子：

```javascript
var returnedNode = someNode.appendChild(newNode);
alert(returnedNode == newNode);         //true
alert(someNode.lastChild == newNode);   //true
```

如果传入到appendChild()中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置转移到新位置。即使可以将DOM树看成是由一系列指针连接起来的，但任何DOM节点也不能同时出现在文档中的多个位置上。因此，如果在调用appendChild()时传入了父节点的第一个子节点，那么该节点就会成为父节点的最后一个子节点，如下面的例子所示：

```javascript
//someNode有多个子节点
var returnedNode = someNode.appendChild(someNode.firstChild);
alert(returnedNode == someNode.firstChild);      //false
alert(returnedNode == someNode.lastChild);       //true
```

如果需要把节点放在childNodes列表中某个特定的位置上，而不是放在末尾，那么可以使用 `insertBefore()` 方法。这个方法接受两个参数：要插入的节点和作为参照的节点。插入节点后，被插入的节点会变成参照节点的前一个同胞节点（previousSibling），同时被方法返回。如果参照节点是null，则insertBefore()与appendChild()执行相同的操作，如下面的例子所示：

```javascript
//插入后成为最后一个子节点
returnedNode = someNode.insertBefore(newNode, null);
alert(newNode == someNode.lastChild);   //true

//插入后成为第一个子节点
var returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
alert(returnedNode == newNode);         //true
alert(newNode == someNode.firstChild);  //true

//插入到最后一个子节点前面
returnedNode = someNode.insertBefore(newNode, someNode.lastChild);
alert(newNode == someNode.childNodes[someNode.childNodes.length-2]); //true
```

`replaceChild()` 方法接受的两个参数是：要插入的节点和要替换的节点。要替换的节点将由这个方法返回并从文档树中被移除，同时由要插入的节点占据其位置。来看下面的例子：

```javascript
//替换第一个子节点
var returnedNode = someNode.replaceChild(newNode, someNode.firstChild);

//替换最后一个子节点
returnedNode = someNode.replaceChild(newNode, someNode.lastChild);
```

在使用replaceChild()插入一个节点时，该节点的所有关系指针都会从被它替换的节点复制过来。尽管从技术上讲，被替换的节点仍然还在文档中，但它在文档中已经没有了自己的位置。

如果只想移除而非替换节点，可以使用 `removeChild()` 方法。这个方法接受一个参数，即要移除的节点。被移除的节点将成为方法的返回值，如下面的例子所示：

```javascript
//移除第一个子节点
var formerFirstChild = someNode.removeChild(someNode.firstChild);

//移除最后一个子节点
var formerLastChild = someNode.removeChild(someNode.lastChild);
```

上面介绍的四个方法操作的都是某个节点的子节点，也就是说，要使用这几个方法必须先取得父节点（使用parentNode属性）。另外，并不是所有类型的节点都有子节点，如果在不支持子节点的节点上调用了这些方法，将会导致错误发生。

另外还有两个方法是所有类型的节点都有的，`cloneNode()`与`normalize()`。cloneNode()用于创建调用这个方法的节点的一个完全相同的副本。cloneNode()方法接受一个布尔值参数，表示是否执行深复制。在参数为true的情况下，执行深复制，也就是复制节点及其整个子节点树；在参数为false的情况下，执行浅复制，即只复制节点本身。复制后返回的节点副本属于文档所有，但并没有为它指定父节点。因此，这个节点副本就成为了一个“孤儿”，除非通过appendChild()、insertBefore()或replaceChild()将它添加到文档中。例如，假设有下面的HTML代码：

```html
<ul>
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3</li>
</ul>
```

如果我们已经将`<ul>`元素的引用保存在了变量myList中，那么通常下列代码就可以看出使用cloneNode()方法的两种模式：

```javascript
var deepList = myList.cloneNode(true);
alert(deepList.childNodes.length);      //3（IE < 9）或7（其他浏览器）－差异主要是因为IE8及更早版本与其他浏览器处理空白字符的方式不一样。IE9之前的版本不会为空白符创建节点。

var shallowList = myList.cloneNode(false);
alert(shallowList.childNodes.length);   //0
```

normalize()方法唯一的作用就是处理文档树中的文本节点。由于解析器的实现或DOM操作等原因，可能会出现文本节点不包含文本，或者接连出现两个文本节点的情况。当在某个节点上调用这个方法时，就会在该节点的后代节点中查找上述两种情况。如果找到了空文本节点，则删除它；如果找到相邻的文本节点，则将它们合并为一个文本节点。本章后面还将进一步讨论这个方法。

* * *

#### `Document` 类型

JavaScript通过Document类型表示文档。在浏览器中，document对象是HTMLDocument（继承自Document类型）的一个实例，表示整个HTML页面。而且，document对象是window对象的一个属性，因此可以将其作为全局对象来访问。  

Document类型可以表示HTML页面或者其他基于XML的文档。不过，最常见的应用还是作为HTMLDocument实例的document对象。通过这个文档对象，不仅可以取得与页面有关的信息，而且还能操作页面的外观及其底层结构。

##### **文档子节点**

document对象的 documentElement属性始终指向HTML页面中的 `<html>`元素（文档元素）；
```javascript
var html = document.documentElement;        //取得对<html>的引用

//也可以通过childNodes列表访问文档元素，不过效率稍差一点
console.log(html === document.childNodes[0]);     //true
console.log(html === document.firstChild);        //true
```

作为HTMLDocument的实例，document对象还有一个body属性，直接指向`<body>`元素。
```javascript
var body = document.body;    //取得对<body>的引用
```

所有浏览器都支持document.documentElement和document.body属性。

##### **文档信息**

作为HTMLDocument的一个实例，document对象还有一些标准的Document对象所没有的属性。这些属性提供了document对象所表现的网页的一些信息。  

  - `title` 属性：可写，包含着`<title>`元素中的文本；修改title属性的值会改变`<title>`元素内容；
  - `URL` 属性：只读，包含页面完整的URL（即地址栏中显示的URL）；
  - `domain` 属性：可写，只包含页面的域名；
  - `referrer`属性：只读，保存着链接到当前页面的那个页面的URL。

后3个属性均与对网页的请求有关，其中只有 `domain` 属性是可设置的。但由于安全方面的限制，也并非可以给domain设置任何值，如果URL中包含一个子域名，例如p2p.wrox.com，那么就只能将domain设置为"wrox.com"。同时览器对domain属性还有一个限制，即如果域名一开始是“松散的”（loose），那么不能将它再设置为“紧绷的”（tight）。换句话说，在将document.domain设置为"wrox.com"之后，就不能再将其设置回"p2p.wrox.com"，否则将会导致错误。

将同一个域名的两个不同的子域名的页面的 `domain` 属性设置为它们共有的父级域名，可以实现`跨域`。

##### **查找元素** 

Document类型提供了两个查找元素的方法：

  - `getElementById()`
  - `getElementsByTagName()`。  

第三个方法，也是只有 `HTMLDocument` 类型才有的方法，是 `getElementsByName()`，最常用于取得单选按钮的情况。  

`getElementsByTagName()` 和 `getElementsByName()` 方法返回一个 HTMLCollection 对象，可以使用方括号语法或item()方法来访问HTMLCollection对象中的项，还支持按名称访问其中的项。另外，该对象还有一个 `nameItem()` 方法，可通过元素的 name 属性取得集合中的项。 
例如，对于以下HTML
```html
<body>
  <img src="#" alt="picture">
  <img src="#" alt="picture" name="myImg">
  <img src="#" alt="picture">
</body>
```

```javascript
var images = document.getElementsByTagName('img');
alert(images[1] === images.item(1));                  // true
alert(images[1] === images["myImg"]);                 // true
alert(images[1] === images.nameItem("myImg"));          // true
```

##### **特殊集合**

除了属性和方法，`document` 对象还有一些特殊的集合。这些集合都是 HTMLCollection 对象。为访问文档常用的部分提供了快捷方式，包括： 

  - `document.anchors`，包含文档中所有带name特性的`<a>`元素；
  - `document.links`，包含文档中所有带href特性的`<a>`元素。
  - `document.forms`，包含文档中所有的`<form>`元素，与`document.getElementsByTagName("form")`得到的结果相同；
  - `document.images`，包含文档中所有的`<img>`元素，与`document.getElementsByTagName("img")`得到的结果相同；

这个特殊集合始终都可以通过HTMLDocument对象访问到，而且，与HTMLCollection对象类似，集合中的项也会随着当前文档内容的更新而更新。

##### **DOM一致性检测**

由于DOM分为多个级别，也包含多个部分，因此检测浏览器实现了DOM的哪些部分就十分必要了。`document.implementation` 属性就是为此提供相应信息和功能的对象，与浏览器对DOM的实现直接对应。DOM1级只为 `document.implementation` 规定了一个方法，即 `hasFeature()`。这个方法接受两个参数：要检测的DOM功能的名称及版本号。如果浏览器支持给定名称和版本的功能，则该方法返回true，如下面的例子所示：

```javascript
var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
```

##### **文档写入**

有一个document对象的功能已经存在很多年了，那就是将输出流写入到网页中的能力。这个能力体现在下列4个方法中：

  - write()，接受一个字符串参数，即要写入到输出流中的文本，原样写入；
  - writeln()，接受一个字符串参数，即要写入到输出流中的文本，写入时会在字符串的末尾添加一个换行符 `\n`；
  - open()，打开网页的输出流；
  - close()，关闭网页的输出流；


#### `Element` 类型

除了Document类型之外，Element类型就要算是Web编程中最常用的类型了。Element类型用于表现XML或HTML元素，提供了对元素标签名、子节点及特性的访问。Element节点具有以下特征：
 
  - nodeType 的值为1；
  - nodeName 的值为元素的标签名；
  - nodeValue 的值为 null；
  - parentNode 可能是 Document 或 Element；
  - 其子节点可能是Element、Text、Comment、ProcessingInstruction、CDATASection或EntityReference。

要访问元素的标签名，可以使用 `nodeName`属性，也可以使用 `tagName`属性；这两个属性会返回相同的值（使用后者主要是为了清晰起见）。
在HTML中，标签名始终都以全部*大写*表示；而在XML（有时候也包括XHTML）中，标签名则始终会与源代码中的保持一致。假如你不确定自己的脚本将会在HTML还是XML文档中执行，最好是在比较之前将标签名转换为相同的大小写形式。如：
```javascript
if (element.tagName.toLowerCase() == "div"){ 
  //这样最好（适用于任何文档）    
  //在此执行某些操作
}
```

##### **HTML元素**

所有HTML元素都由HTMLElement类型表示，不是直接通过这个类型，也是通过它的子类型来表示。HTMLElement类型直接继承自Element并添加了一些属性。添加的这些属性分别对应于每个HTML元素中都存在的下列标准特性。
 
  - id，元素在文档中的唯一标识符。
  - title，有关元素的附加说明信息，一般通过工具提示条显示出来。
  - lang，元素内容的语言代码，很少使用。
  - dir，语言的方向，值为"ltr"（left-to-right，从左至右）或"rtl"（right-to-left，从右至左），也很少使用。
  - `className`，与元素的class特性对应，即为元素指定的CSS类。没有将这个属性命名为class，是因为class是ECMAScript的保留字。

##### **取得特性**

每个元素都有一或多个特性，这些特性的用途是给出相应元素或其内容的附加信息。操作特性的DOM方法主要有三个：  

  - getAttribute('attr_name')
  - setAttribute('attr_name', 'attr_value')
  - removeAttribute('attr_name')

这三个方法可以针对任何特性使用，包括那些以HTMLElement类型属性的形式定义的特性。

> 注意，传递给getAttribute()的特性名与实际的特性名相同。因此要想得到class特性值，应该传入"class"而不是"className"，后者只有在通过对象属性访问特性时才用。如果给定名称的特性不存在，getAttribute()返回null。

特性名不区分大小写。
任何元素的所有特性，也都可以通过DOM元素本身的属性来访问。当然，HTMLElement也会有5个属性与相应的特性一一对应。不过，只有公认的（非自定义的）特性才会以属性的形式添加到DOM对象中。因而操作DOM时，开发人员一般不使用getAttribute()，而是只使用对象的属性。只有在取得自定义特性值的情况下，才会使用getAttribute()方法。

##### **attributes属性**

Element类型是使用 `attributes`属性的唯一一个DOM节点类型。
每个特性节点都有一个名为 `specified` 的属性，这个属性的值如果为true，则意味着要么是在HTML中指定了相应特性，要么是通过setAttribute()方法设置了该特性。

**遍历元素的特性**
```javascript
function outputAttributes(element){
  var pairs = new Array(),
      attrName,
      attrValue,
      i,
      len;
  for (i=0, len=element.attributes.length; i < len; i++){
      attrName = element.attributes[i].nodeName;
      attrValue = element.attributes[i].nodeValue;
      if (element.attributes[i].specified) {
          pairs.push(attrName + "=\"" + attrValue + "\"");
        }
      }
  return pairs.join(" ");
}
```

##### **创建元素**

使用 `document.createElement()` 方法可以创建新元素。这个方法只接受一个参数，即要创建元素的标签名。这个标签名在HTML文档中不区分大小写，而在XML（包括XHTML）文档中，则是区分大小写的。

在IE中可以以另一种方式使用createElement()，即为这个方法传入完整的元素标签，也可以包含属性，如下面的例子所示。

```javascript
var div = document.createElement("<div id=\"myNewDiv\" class=\"box\"></div >");
```

这种方式有助于避开在IE7及更早版本中动态创建元素的某些问题。

##### **元素子节点**

如果需要通过childNodes属性遍历子节点，通常都要先检查一下nodeTpye属性，如下面的例子所示。

``` javascript
for (var i=0, len=element.childNodes.length; i < len; i++){
  if (element.childNodes[i].nodeType == 1){
    //执行某些操作
  }
}
```

如果想通过某个特定的标签名取得子节点或后代节点该怎么办呢？  
实际上，元素也支持 `getElementsByTagName()` 方法。  
在通过元素调用这个方法时，除了搜索起点是当前元素之外，其他方面都跟通过document调用这个方法相同，因此结果只会返回当前元素的后代。例如，要想取得前面 `<ul>` 元素中包含的所有 `<li>` 元素，可以使用下列代码。

```javascript
var ul = document.getElementById("myList");
var items = ul.getElementsByTagName("li");
```

* * *

#### `Text` 类型

文本节点由Text类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的HTML字符，但不能包含HTML代码。Text节点具有以下特征：

  - nodeType的值为3；
  - nodeName的值为"#text"；
  - `nodeValue` 的值为节点所包含的文本；
  - parentNode是一个Element；
  - 不支持（没有）子节点。

可以通过 `nodeValue` 属性或 `data` 属性访问 Text 节点中包含的文本，这两个属性中包含的值相同。对 `nodeValue` 的修改也会通过 `data` 反映出来，反之亦然。使用下列方法可以操作节点中的文本：

  - `appendData(text)`：将text添加到节点的末尾。
  - `deleteData(offset, count)`：从offset指定的位置开始删除count个字符。
  - `insertData(offset, text)`：在offset指定的位置插入text。
  - `replaceData(offset, count, text)`：用text替换从offset指定的位置开始到offset+ count为止处的文本。
  - `splitText(offset)`：从offset指定的位置将当前文本节点分成两个文本节点。
  - `substringData(offset, count)`：提取从offset指定的位置开始到offset+count为止处的字符串。

除了这些方法之外，文本节点还有一个 `length` 属性，保存着节点中字符的数目。而且，`nodeValue.length` 和 `data.length` 中也保存着同样的值。  
在默认情况下，每个可以包含内容的元素最多只能有一个文本节点，而且必须确实有内容存在。

**访问与修改文本子节点**
```html
<div>Hellow World!</div>

<script>
  var div = document.getElementsByTagName('div')[0];
  var textNode = div.firstChild;  //取得文本节点
  div.firstChild.nodeValue = "Some other messages";   //修改文本节点
  div.firstChild.nodeValue = "Some <strong>other</strong> messages";  //输出结果会是"Some &lt;strong&gt;other&lt;/strong&gt; messages"
</script>
```

> 在修改文本节点时还要注意，此时的字符串会经过**HTML（或XML，取决于文档类型）编码**。换句话说，小于号、大于号或引号都会像上面的例子一样被转义。  

##### **创建文本节点**

可以使用 `document.createTextNode()` 创建新文本节点，这个方法接受一个参数——要插入节点中的文本。与设置已有文本节点的值一样，作为参数的文本也将按照HTML或XML的格式进行编码。

```javascript
var textNode = document.createTextNode("<strong>Hello</strong> world!"); //输出结果会是"&lt;strong&gt;Hello&lt;/strong&gt; world!"
element.appendChild(textNode);  //通过 appendChild() 方法，将文本节点添加到元素节点内
```


##### **规范文本节点**

一般情况下，每个元素只有一个文本子节点。不过，在某些情况下也可能包含多个文本子节点。这时一般会用 `normalize()`方法来规范化。
如果在一个包含两个或多个文本节点的父元素上调用normalize()方法，则会将所有文本节点合并成一个节点，结果节点的nodeValue等于将合并前每个文本节点的nodeValue值拼接起来的值。

```javascript
var element = document.createElement("div");
element.className = "message";
var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);
var anotherTextNode = document.createTextNode("Yippee!");
element.appendChild(anotherTextNode);document.body.appendChild(element);
alert(element.childNodes.length);    //2

element.normalize();
alert(element.childNodes.length);    //1
alert(element.firstChild.nodeValue);    // "Hello world!Yippee!"
```

##### **分割文本节点**

Text类型提供了一个作用与normalize()相反的方法：

  - `splitText()`   

这个方法会将一个文本节点分成两个文本节点，即按照指定的位置分割 `nodeValue` 值。原来的文本节点将包含从开始到指定位置之前的内容，新文本节点将包含剩下的文本。这个方法会返回一个新文本节点，该节点与原节点的 `parentNode` 相同。来看下面的例子。

```javascript
var element = document.createElement("div");
element.className = "message";
var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);document.body.appendChild(element);
var newNode = element.firstChild.splitText(5);  //从索引位置“5”(即空格)前切一刀，分成两个文本节点，返回后面的文本节点
alert(element.firstChild.nodeValue);    //"Hello"
alert(newNode.nodeValue);               //" world!"
alert(element.childNodes.length);       //2
```

分割文本节点是从文本节点中提取数据的一种常用DOM解析技术。

<!-- * * *

#### `Comment` 类型

#### `CDATASection` 类型

#### `DocumentType` 类型 -->

* * *

#### `DocumentFragment` 类型

在所有节点类型中，只有DocumentFragment在文档中没有对应的标记。DOM规定文档片段（document fragment）是一种“轻量级”的文档，可以包含和控制节点，但不会像完整的文档那样占用额外的资源。DocumentFragment节点具有下列特征：
 
  - nodeType的值为11；
  - nodeName的值为"#document-fragment"；
  - nodeValue的值为null；
  - parentNode的值为null；
  - 子节点可以是Element、ProcessingInstruction、Comment、Text、CDATASection或EntityReference。  

虽然不能把文档片段直接添加到文档中，但可以将它作为一个“仓库”来使用，即可以在里面保存将来可能会添加到文档中的节点。  

要创建文档片段，可以使用 `document.createDocumentFragment()` 方法。

文档片段继承了Node的所有方法，通常用于执行那些针对文档的DOM操作。*如果将文档中的节点添加到文档片段中，就会从文档树中移除该节点*，也不会从浏览器中再看到该节点。添加到文档片段中的新节点同样也不属于文档树。可以通过appendChild()或insertBefore()将文档片段中内容添加到文档中。在将文档片段作为参数传递给这两个方法时，实际上只会将文档片段的所有子节点添加到相应位置上；文档片段本身永远不会成为文档树的一部分。

来看下面的HTML示例代码：

```html
<ul id="myList"></ul>
```

假设我们想为这个`<ul>`元素添加3个列表项。如果逐个地添加列表项，将会导致浏览器**反复渲染**（呈现）新信息。为**避免**这个问题，可以像下面这样使用一个文档片段来保存创建的列表项，然后再一次性将它们添加到文档中。

```javascript
var fragment = document.createDocumentFragment();
var ul = document.getElementById("myList");
var li = null;
for (var i=0; i < 3; i++){
    li = document.createElement("li");
    li.appendChild(document.createTextNode("Item " + (i+1)));
    fragment.appendChild(li);
}
ul.appendChild(fragment);
```

#### `Attr` 类型


* * *

### DOM操作技术

#### 动态脚本

**加载外部脚本文件**
```javascript
function loadScript(url) {
  var script = document.createElement('script');
  script.type = "text/javascript";
  script.src = url;
  document.body.appendChild(script);
}

loadScript("client.js");
```

**加载行内脚本**
```javascript
function loadScriptString(code) {
  var script = document.createElement('script');
  script.type = "text/javascript";
  try {
    script.appendChild(document.createTextNode(code));
  } catch (ex){
    script.text = code;   // 兼容IE
  }
  document.body.appendChild(script);
}

loadScriptString("function sayHi(){alert('hi');}");
```

#### 动态样式

**加载外部样式文件**
```javascript
function loadStyles(url) {
  var link = document.createElement('link');
  link.rel = "stylesheet";
  link.type = "text/css";
  link.href = url;

  var head = getElementsByTagName('head')[0];
  head.appendChild(link);
}

loadStyles("styles.css");
```

**加载行内样式**
```javascript
function loadStylesString(css) {
  var style = document.createElement('style');
  style.type = "text/css";

  try{
    style.appendChild(document.createTextNode(css))
  } catch (ex){
    style.styleSheet.cssText = css;  //兼容IE
  }

  var head = getElementsByTagName('head')[0];
  head.appendChild(style);
}

loadStylesString("body{background-color:#333;}");
```


#### 操作表格

用核心DOM方法创建表格通常代码很长且不太清晰，因而为了方便构建表格，HTML、DOM还为 `<table>`、`<tbody>`和`<tr>`元素添加了一些属性和方法。

  - 为 `<table>` 元素添加的属性和方法如下:  
    - `caption`：保存着对 `<caption>` 元素（如果有）的指针。
    - `tBodies`：是一个 `<tbody>` 元素的 HTMLCollection。
    - `tFoot`：保存着对 `<tfoot>` 元素（如果有）的指针。
    - `tHead`：保存着对 `<thead>` 元素（如果有）的指针。
    - `rows`：是一个表格中所有行的 HTMLCollection。
    - `createTHead()`：创建 `<thead>` 元素，将其放到表格中，返回引用。
    - `createTFoot()`：创建 `<tfoot>` 元素，将其放到表格中，返回引用。
    - `createCaption()`：创建 `<caption>` 元素，将其放到表格中，返回引用。
    - `deleteTHead()`：删除<thead>元素。
    - `deleteTFoot()`：删除<tfoot>元素。
    - `deleteCaption()`：删除<caption>元素。
    - `deleteRow(_pos_)`：删除指定位置的行。
    - `insertRow(_pos_)`：向 `rows` 集合中的指定位置插入一行。

  - 为`<tbody>`元素添加的属性和方法如下：  
    - `rows`：保存着`<tbody>`元素中行的HTMLCollection。
    - `deleteRow(pos)`：删除指定位置的行。
    - `insertRow(pos)`：向rows集合中的指定位置插入一行，返回对新插入行的引用。

  - 为`<tr>`元素添加的属性和方法如下：  
    - `cells`：保存着 `<tr>` 元素中单元格的 HTMLCollection。
    - `deleteCell(pos)`：删除指定位置的单元格。
    - `insertCell(pos)`：向 `cells` 集合中的指定位置插入一个单元格，返回对新插入单元格的引用。

**生成一个两行三列表格**
```javascript
//创建表格元素
var table = document.createElement('table');
table.border=1;
table.width = '100%';

//创建tbody
var tbody = document.createElement('tbody');
table.appendChild(tbody);

//创建第一行
tbody.insertRow(0);
tbody.rows[0].insertCell(0);
tbody.rows[0].insertCell(1);
tbody.rows[0].insertCell(2);

tbody.rows[0].cells[0].appendChild(document.createTextNode('1.1'));
tbody.rows[0].cells[1].appendChild(document.createTextNode('1.2'));
tbody.rows[0].cells[2].appendChild(document.createTextNode('1.3'));

//创建第二行
tbody.insertRow(1);
tbody.rows[1].insertCell(0);
tbody.rows[1].insertCell(1);
tbody.rows[1].insertCell(2);

tbody.rows[1].cells[0].appendChild(document.createTextNode('2.1'));
tbody.rows[1].cells[1].appendChild(document.createTextNode('2.2'));
tbody.rows[1].cells[2].appendChild(document.createTextNode('2.3'));

//将表格添加到页面中
document.body.appendChild(table);
```

<!-- #### 使用NodeList -->

### 小结

DOM是语言中立的API，用于访问和操作HTML和XML文档。DOM1级将HTML和XML文档形象地看作一个层次化的节点树，可以使用JavaScript来操作这个节点树，进而改变底层文档的外观和结构。

DOM由各种节点构成，简要总结如下：  

  - 最基本的节点类型是 `Node`，用于抽象地表示文档中一个独立的部分；所有其他类型都继承自Node。
  - `Document` 类型表示整个文档，是一组分层节点的根节点。在 JavaScript 中，`document` 对象是 `Document` 的一个实例。使用 `document` 对象，有很多种方式可以查询和取得节点。
  - `Element` 节点表示文档中的所有 HTML或XML 元素，可以用来操作这些元素的内容和特性。
  - 另外还有一些节点类型，分别表示文本内容、注释、文档类型、CDATA区域和文档片段。

访问 DOM 的操作在多数情况下都很直观，不过在处理 `<script>`和`<style>` 元素时还是存在一些复杂性。由于这两个元素分别包含脚本和样式信息，因此浏览器通常会将它们与其他元素区别对待。这些区别导致了在针对这些元素使用innerHTML时，以及在创建新元素时的一些问题。  

理解DOM的关键，就是理解DOM对性能的影响。DOM操作往往是JavaScript程序中开销最大的部分，而因访问NodeList导致的问题为最多。NodeList对象都是“动态的”，这就意味着每次访问NodeList对象，都会运行一次查询。有鉴于此，最好的办法就是尽量减少DOM操作。