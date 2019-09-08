## 第12章、DOM2 和 DOM3

本章内容

> DOM2 和 DOM3 的变化
>
> 操作样式的 DOM API
>
> DOM 遍历与范围

DOM1 级主要定义的是文档的底层结构，DOM2 和 DOM3 描述的是具体的子集，引入了更多的方法、属性修改 DOM。

### 12.1 DOM 变化

针对 XML 命名空间进行了扩展。

类型、框架方面做了一些变化。

> 由于一般是通过元素访问特性，这一块基本很少用。

访问 DOM2 级可以通过下面的方式：

```js
 document.implementation;
// 判断浏览器是否支持 DOM2 级核心
document.implementation.hasFeature('Core', '2.0'); // true / false
// 判断浏览器是否支持 DOM3 级核心
document.implementation.hasFeature('Core', '3.0'); // true / false
```



### 12.2	样式

判断浏览器是否支持 DOM2 级定义的 CSS 能力:

```js
document.implementation.hasFeature('css', '2.0'); // true / false
```

访问 CSS 属性时，类似`background-image`这种短划线的词汇需要转化为驼峰命名形式访问

```js
// 访问元素的 background-image 属性
$ele.backgroundImage; // 不能直接写 $ele.background-image
```

另外，针对一些 JS 的保留关键字，比如 `float`，其属性名是 `cssFloat`。

DOM2 级提供了一些属性和方法，可以修改样式：

- cssText，获取 style 特性的 CSS 代码。
- removeProperty(propName)，从 style 特性中移除指定样式属性。

操作元素样式最方便的是使用 `$ele.style`，结合`removeProperty()`方法删除指定样式。

#### 12.2.3 元素大小

**偏移量**，是包含元素在屏幕上占用的所有可见的空间

> **offsetHeight**，元素在垂直方向上占用的空间大小，以像素计。包含元素的高度、可见的垂直滚动条的高度、上下边框的高度。
>
> **offsetWidth**，元素在水平方向上占用的空间大小，以像素计。包含元素的宽度、可见的垂直滚动条的宽度、上下边框的宽度。
>
> **offsetLeft**，元素的左外边框至包含元素的左内边框之间的像素距离。
>
> **offsetTop**，元素的上外边框至包含元素的上内边框之间的像素距离。

这些属性是只读的，每次访问都会重新计算，所以尽量不要频繁访问。没有`offsetTop`、`offsetBottom`。

**客户区大小**，是元素内容及其内边距所占据的空间大小。

> **clientWidth**，元素内容区宽度加上左右内边距宽度。
>
> **clientHeight**，元素内容区高度加上上下内边距宽度。

另外还提到了滚动大小、确定元素大小。





