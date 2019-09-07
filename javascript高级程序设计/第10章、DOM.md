## 第10章、DOM

本章内容

> 理解包含不同层次节点的 DOM
>
> 使用不同的节点类型
>
> 客服浏览器兼容性问题及各种陷阱

DOM（文档对象模型）描绘了一个层次化的节点树，允许开发人员添加、删除和修改页面的某一部分。

### 10.1 节点层次

DOM 可以将任何 HTML 或 XML 文档描绘成一个由多层节点构成的结构。节点分为不同的类型，每个节点都有各自的特点、数据和方法，与其他节点存在某种关系。节点之间的关系构成了层次，然后以特定节点为根节点构成树形结构。

以下面的 THML 结构为例：

```html
<html>
  <head>
 	<title>Sample Page</title>
  </head>
  <body>
 	<p>Hello World!</p>
  </body>
</html> 
```

其节点构成的树形结构如下：

```
Document
|__Element html
	|__Element head
	|	|__Element title
	|		|__Text Sample Page
	|
	|__Element body
		|__Element p
			|__Text Hello World!
```

文档节点是每个文档根节点。HTML 文档中一般只有一个根节点 <html> 元素，我们称之为文档元素。文档元素是文档的最外层元素，文档中的其他所有元素都包含在文档元素中。

总共有12种节点类型，这些类型都继承自一个基类型。

#### 10.1.1 Node 类型

DOM1级定义了一个 Node 接口，该接口由 DOM 中的所有节点类型实现。该接口在 JS 中是作为 Node 类型实现的，所有节点类型都共享相同的基本属性和方法。

> IE 无法访问该 Node 类型常量

每个节点都要一个 nodeType 属性，用于表明节点类型。有以下12个类型：

- Node.ELEMENT_NODE(1)
- Node.ATTRIBUTE_NODE(2)
- Node.TEXT_NODE(3)
- Node.CDATA_SECTION_NODE(4)
- Node.ENTITY_REFERENCE_NODE(5)
- Node.ENTITY_NODE(6)
- Node.PROCESSING_INSTRUCTION_NODE(7)
- Node.COMMENT_NODE(8)
- Node.DOCUMENT_NODE(9)
- Node.DOCUMENT_TYPE_NODE(10)
- Node.DOCUMENT_FRAGMENT_NODE(11)
- Node.NOTATION_NODE(12)

通过比较上面的常量，可以判断节点是什么类型

```js
if (someNode.nodeType == Node.ELEMENT_NODE){ // IE 下无效
 	alert("Node is an element.");
} 

// IE 下不能访问 Node 类型常量，但是可以通过数值比较
if (someNode.nodeType == 1){ // 适用所有浏览器
 	alert("Node is an element.");
} 
```

获取节点的名称和值

```js
someNode.nodeName; // 元素节点的 nodeName 始终返回元素的标签名，比如<div>元素返回"DIV"
someNode.nodeValue; // 元素节点的 nodeValue 始终是空的
```

在 HTML 中把节点之间的关系构成的文档树比喻成家谱。在 HTML 中，<html> 是唯一跟节点，作为父元素，<body> 和 <head> 是他的子元素，而 <body> 和 <head> 则是同胞元素，因为他们是 <html> 的直接子元素。

每个节点都有一个 `childNodes` 属性，其中保存着一个 `NodeList` 对象。`NodeList` 保存一组有序的节点，可以通过位置访问这些节点。`NodeList`不是数组。

> `NodeList`是基于 DOM 结构动态执行查询的结果，所以 DOM 结构的变化能够自动反应在这个对象里。

每个节点都有一个`parentNode`属性，该属性指向文档树中的父节点。包含在 `childNodes` 列表中的所有节点都有相同的父节点。`childNodes`列表中每个子节点互为同胞节点。通过子节点的`previousSibling`和`nextSibling`属性可以访问子节点的前后节点。

> 当该节点没有前后节点时，`previousSibling`和`nextSibling`返回 null

父节点可以通过`firstChild`和`lastChild`访问节点列表中的第一个和最后一个节点。等价于`childNodes[0]`和`childNodes[childNodes.length - 1]`。

`hasChildNodes()` 可以快捷判断是否拥有子节点。

所有节点都有一个属性`ownerDocument`，该属性指向表示整个文档的文档节点。

#### 10.1.2 Document 类型

nodeType为0;

nodeName为"#document";

nodeValue、parentNode、ownerDocument都为null。

JS 通过 Document 类型表示文档。

可以通过 `document.documentElement` 或 `document.childNodes[0]`快速访问根节点 <html> 元素。

可以通过 `document.body`快速访问 <body> 元素。

可以通过 `document.doctype` 取得 `<!DOCTYPE>` 的引用。但是浏览器对这个属性支持不一。

不同浏览器对于 HTML 文档中的注释是否处理为节点有不同的处理。

#### 10.1.3 Element 类型

Element 节点具有以下特点：

- nodeType 为1
- nodeName 的值为元素的标签名
- nodeValue 为 null
- parentNode 可能是 Document 或 Element

访问元素的标签名，可以用 nodeName 属性，也可以用 tagName 属性。在 HTML 文档中，标签名始终是大写的。

```js
element.tagName.toLowerCase() == "div"; // 这样比较才能得出正确结果，或者把右边的字符串转为大写
```

最常见的元素比如 `html`、`div`、`ul`等，他们都有以下标准特性：

- id，元素在文档中的唯一标识
- title，有关元素的附加说明信息，一般通过浏览器默认的提示条展示。
- className，与元素的 class 特性对应，非唯一标识。

这些标准特性都可以获取或修改

```js
var $ele = document.getElementById('test');
// 获取
alert($ele.id);
alert($ele.title);
alert($ele.className);
// 修改
$ele.id = 'test1';
$ele.title = '哈哈哈';
$ele.className = 'div1';
```

对 id 的修改，如果没有关联样式，不会立即反应到页面上；对 title 的修改需要鼠标移动到元素上才会反应；对 className的修改会立即反应到页面上。

所有 HTML 元素都是由 HTMLElment 或者其更具体的子类型表示的，比如 `DIV` 更具体的子类型为 `HTMLDivElement`。

可以通过 `getAttribute(prop)` 方法获取具体的属性值

```js
$ele.getAttribute('id'); // test
```

特性的名称不区分大小写，也可以获取自定义的特性。

可以通过`setAttribute(prop, value)`方法设置特性

```js
$ele.setAttribute('id', 'test2');
```

可以通过`removeAttribute(prop)`方法删除特性

```js
$ele.removeAttribute('class'); // 整个 class 特性被删除
```

##### attributes 属性

Element 类型是使用 attributes 属性的唯一节点类型。

attributes 属性包含一个 NameNodeMap，与 NodeList 类似，是一个“动态”集合。NameNodeMap 对象拥有下列方法：

- getNamedItem(name)，返回 nodeName 属性等于 name 的节点
- removeNamedItem(name)，从列表中移除 nodeName 为 name 的节点
- setNamedItem(node)，向列表中添加节点，以节点的 nodeName 属性为索引
- item(pos)，返回位于数字 pos 位置处的节点

```js
$ele.attributes.getNamedItem('id').nodeValue; // 获取节点值，等价于 $ele.getAttribute('id'); 
```

attributes 的NameNodeMap 对象拥有的这几个方法在使用上不如 `getAttribute()`这几个方法便捷，attributes 最佳的用途是拿来**遍历元素的特性**。

使用`document.createElement()`可以创建元素，这个方法用处很大，现在最流行的几大框架在实现虚拟 DOM 时，都用到了这个方法：

```js
let element = document.createElement(tagName[, options]);
```

#### 10.1.4 Text 类型

文本节点由 Text 类型表示，是纯文本。纯文本可以包含转义后的 HTML 字符，但不能保护 HTML 代码。

Text 节点有以下特征：

- nodeType 为3
- nodeName 为"#text"
- nodeValue 为节点所包含的文本
- parentNode 是一个 Element
- 没有子节点

可以通过 nodeValue 属性或 data 属性访问 Text 节点中包含的文本，这两个属性包含的内容一样。

原生提供的几种方法有些繁琐，其实对文本节点的操作可以用下面的更快捷

```js
$ele.text; // 获取文本
$ele.text = '123'; // 修改或赋值文本
```

对于文本的切割之类，可以在拿到文本后，用`split()`等方法操作。

#### 10.1.5	Comment 类型

Comment 即注释，有以下特征：

- nodeType 为8
- nodeName 为"#comment"
- nodeValue 是注释的内容
- parentNode 可能是 Document 或 Element
- 没有子节点

Comment 类型与 Text 类型继承自相同的基类，所以他们通用除 `splitText()`之外的所有方法，都通过 nodeValue 属性或 data 属性访问内容。

#### 10.1.6 CDATASection 类型

CDATASection 类型只针对 XML 文档，表示的是 CDATA 区域。该类型也继承自 Text 类型，基本与 Text、Comment一样。

其 nodeType 为4。

#### 10.1.7 DocumentType 类型

这个类型是拿 doctype 有关的信息，其 nodeType 为 10。

在 HTML5 时代，这个类型基本不会在 JS 中操作。

#### 10.1.8 DocumentFragment 类型

这个类型是一种“轻量级”的文档，可以包含和控制节点，但不会像完整的文档那样占用额外的资源。

其 nodeType 为11。

#### 10.1.9 Attr 类型

这个类似是元素 Element 的特性，其 nodeType 为2。存在与元素的 attributes 属性。具体的在 Element 类型讲到了。

### 10.2 DOM 操作技术

这一章节重点提到了怎么操作 table，提到了表格的个属性和方法。

搜MDN的时候，看到了一个有意思的 console

```js

function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

var john = new Person("John", "Smith");
var jane = new Person("Jane", "Doe");
var emily = new Person("Emily", "Jones");

console.table([john, jane, emily]);
```

`console.table` 可以在控制台将数据以表格的形式打印出来。IE 不支持这个方法。



### 总结

- DOM 是语言中立的 API
- DOM 由各种节点组成，最基本的节点类型是 Node
- DOM 操作是 JS 中开销最大的、最损耗性能的。所以才会出现 JQuery、React、Vue等等。

这一章讲的很基础，主要讲到了 DOM 的基本类型，怎么操作各种基本类型。