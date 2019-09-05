## 第8章、BOM

本章内容

> 理解 window 对象——BOM核心
>
> 控制窗口、框架和弹出窗口
>
> 利用 location 对象中的页面信息
>
> 使用 navigator 对象了解浏览器

### 8.1 window 对象

BOM 的核心对象是 window，它表示浏览器的一个实例。window 既是操作浏览器窗口的一个接口，又是 ES 规定的 Global 对象。所以可以通过 window 访问所有全局定义的对象、变量和函数。

#### 8.1.1 全局作用域

由于 window 对象同时扮演 ES 中 Global 对象的角色，所以可以通过 window 访问所有全局定义的对象、变量和函数。

```js
var age = 29;
function sayAge(){console.log(age);}
window.age; // 29
sayAge(); // 29
window.sayAge(); // 29
```

#### 8.1.2 窗口关系及框架

如果页面中包含框架 `frame`，则每个框架都拥有自己的 window 对象，并且保存在 frames 集合中。在 frames 集合中，可以通过数值索引（从0开始，从左往右，从上往下）或者框架名称来访问相应的 window 对象。

有几个特殊的对象属性可以访问对应的 window 对象

> `top`，该对象始终指向最高层的框架，即浏览器窗口。
>
> `parent`，该对象始终指向当前框架的上一层框架。
>
> `slef`，该对象始终指向当前框架。

在使用框架时，浏览器中会存在多个 Global，框架内定义的变量、属性、函数彼此互不干扰。

#### 8.1.3 窗口位置

浏览器基本提供了两个参数用来确定和修改窗口的位置

> 有的浏览器用这两个：`screenLeft` 和 `screenTop`
>
> 有的浏览器用这两个：`screenX` 和 `screenY`

但是这两个参数的坐标系在不同浏览器中有一些差异，可以通过`moveTo()`和`moveBy()`精确的校准窗口位置。

```js
// 将窗口移动到屏幕左上角
window.moveTo(0, 0);
// 将窗口向下移动100像素
window.moveBy(0, 100);
```

但是这两个方法可能会被浏览器禁用，而且这两个方法都不适用于框架，只能对最外层的 window 对象使用。

#### 8.1.4 窗口大小

获取浏览器窗口本身的尺寸

```js
window.outerWidth;
window.outerHeight;
```

获取浏览器视图区（viewport）大小

```js
// 标准模式
window.innerWidth;
window.innerHeight;

// 混杂模式
document.documentElement.clientWidth;
document.documentElement.clientHeight;
```

改变窗口大小

```js
// 调整到100*100
window.resizeTo(100, 100);
// 调整到200*150
window.resizeBy(100, 50);
```

**注意：** 上面这两个方法可能会被浏览器禁用，同时，这两个方法只能对最外层的 window 对象使用。

#### 8.1.5 导航和打开窗口

通过 window.open() 方法可以打开新窗口

```js
let windowObjectReference = window.open(strUrl, strWindowName, [strWindowFeatures]);
```
参数

> **WindowObjectReference**，打开的新窗口对象的引用。如果调用失败，返回值会是 null 。如果父子窗口满足“同源策略”，可以通过这个引用访问新窗口的属性或方法。
>
> **strUrl**，新窗口需要载入的url地址。strUrl可以是web上的html页面也可以是图片文件或者其他任何浏览器支持的文件格式。
>
> **strWindowName**，新窗口的名称。该字符串可以用来作为超链接 `<a>` 或表单 `<form>` 元素的目标属性值。字符串中不能含有空白字符。注意：strWindowName 并不是新窗口的标题。
>
> > 如有存在一个同名的窗口，则新开的窗口会加载到同名窗口。窗口名可以是特殊的名称：`_self`、`_top`、`_parent`、`_top`、`_blank`
>
> **strWindowFeatures**，可选参数。是一个字符串值，这个值列出了将要打开的窗口的一些特性(窗口功能和工具栏) 。 字符串中不能包含任何空白字符，特性之间用逗号分隔开。
>
> > 该参数包含的特性有：`width`、`height`、`resizable`等等。
> >
> > [具体参考](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open#Position and size features)

`open()` 方法，创建一个新的浏览器窗口对象，如同使用文件菜单中的新窗口命令一样。strUrl参数指定了该窗口将会打开的地址。如果strUrl 是一个空值，那么打开的窗口将会是带有默认工具栏的空白窗口（加载`about:blank`）。

关闭窗口

```js
WindowObjectReference.close();
WindowObjectReference.closed; // true
```

窗口被关闭后，`WindowObjectReference` 引用还存在，但是除了用来检测窗口是否被关闭，已经没什么用了。

#### 8.1.6 间歇调用和超时调用

JavaScript 是单线程语言，但可以通过 `setInterval()` 和 `setTimeout()` 进行间歇调用和超时调用。

```js
var intervalTimer = setInterval(function(){console.log(1)}, 1000); // 1秒执行一次
clearInterval(intervalTimer); // 终止 setInterval() 定时器

var timeoutTimer = setTimeout(function(){console.log(1)}, 1000); // 1秒后执行，执行完就结束定时器
clearTimeout(timeoutTimer); // 提前终止 setTimeout() 定时器
```

#### 8.1.7 系统对话框

浏览器提供了三种对话框：`alert()`、`confirm()`、`prompt()`。

不过现代前端的发展，层出不穷的自定义对话框，系统提供的这三种对话框基本只能在`demo`里出现了。



### 8.2 location 对象

location 是最有用的 BOM 对象之一，既是 window 对象的属性，也是 document 对象的属性。

location 的属性有很多，在兼容性上，有些新增的属性还是不太友好。

#### 属性

[`Location.href`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/href)

包含整个URL的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)

[`Location.protocol`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/protocol)

包含URL对应协议的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，最后有一个":"。

[`Location.host`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/host)

包含了域名的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，可能在该串最后带有一个":"并跟上URL的端口号。

[`Location.hostname`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/hostname)

包含URL域名的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)。

[`Location.port`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/port)

包含端口号的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)。

[`Location.pathname`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/pathname)

包含URL中路径部分的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，开头有一个“`/"。`

[`Location.search`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/search)

 包含URL参数的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，开头有一个`“?”`。

[`Location.hash`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/hash)

包含块标识符的[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，开头有一个`“#”。`

[`Location.username`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/username)

包含URL中域名前的用户名的一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)。

[`Location.password`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/password)

包含URL域名前的密码的一个 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)。

[`Location.origin`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/origin) 只读

包含页面来源的域名的标准形式[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)。

#### 方法

[`Location.assign()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/assign)

加载给定URL的内容资源到这个Location对象所关联的对象上。

[`Location.reload()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/reload)

重新加载来自当前 URL的资源。他有一个特殊的可选参数，类型为 [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/API/Boolean)，该参数为true时会导致该方法引发的刷新一定会从服务器上加载数据。如果是 `false`或没有制定这个参数，浏览器可能从缓存当中加载页面。

[`Location.replace()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/replace)

用给定的URL替换掉当前的资源。与 `assign()` 方法不同的是用 `replace()`替换的新页面不会被保存在会话的历史 [`History`](https://developer.mozilla.org/zh-CN/docs/Web/API/History)中，这意味着用户将不能用后退按钮转到该页面。

[`Location.toString()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/toString)

返回一个[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，包含整个URL。 它和读取[`URLUtils.href`](https://developer.mozilla.org/zh-CN/docs/Web/API/URLUtils/href)的效果相同。但是用它是不能够修改Location的值的。



### 8.3 navigator 对象

navigator 对象是用于检测客户端浏览器的事实标准，所有浏览器都支持，当然属性上会有些差异。

常见的是用 `navigator.userAgent` 检测浏览器版本和类型

```js
// 检测浏览器版本和类型
navigator.userAgent

// 检测插件
navigator.plugins
```



### 8.4 screen 对象

screen 对象只用来表明客户端的能力，其中包括浏览器窗口外部的显示器的信息。这个对象在实际编程中用处不大。



### 8.5 history 对象

history 对象保存着用户上网的历史记录，从窗口打开的那一刻算起。

不同的窗口、标签、框架都有不同的 history。

```js
// 后退一页
history.go(-1);
// 前进一页
history.go(1);
// 前进两页
history.go(2);
// 跳转到百度，如果窗口的历史记录中没有该链接，则无法跳转
history.go('baidu.com');
// 后退一页
history.back();
// 前进一页
history.forward();
```



### 小结

BOM 最常用对象 window 和 location

