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

JS 通过 Document 类型表示文档。

可以通过 `document.documentElement` 或 `document.childNodes[0]`快速访问根节点 <html> 元素。

可以通过 `document.body`快速访问 <body> 元素。

可以通过 `document.doctype` 取得 `<!DOCTYPE>` 的引用。但是浏览器对这个属性支持不一。

不同浏览器对于 HTML 文档中的注释是否处理为节点有不同的处理。

#### 10.1.3 Element 类型



