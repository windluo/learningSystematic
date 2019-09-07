## 第11章、DOM扩展

本章内容

> 理解 Selectors API
>
> 使用 HTML5 DOM 扩展
>
> 了解专有的 DOM 扩展

DOM 扩展即不断的完善、扩展该 API，让人们更加方便的使用。从 ES3 到 ES5，再到最新的大版本 ES6，以及不断更新的小版本 ES7、8、9等等，这么语言在不断的完善。

> W3C 负责将已经成为事实标准的扩展写入规范中。

### 11.1 选择符 API

选择符 API（Selectors API）根据 CSS 选择符选择与某个模式匹配的 DOM 元素。早期的框架，比如 JQuery，提供的快捷 API 底层依然调用的是原生的方法实现，效率并不如直接调用原生的方法，但是直接用原生的 API 要不是很方便。

根据社区不断的发展，原生的 JS 提供了一些快捷的底层方法获取 DOM 原生，最常用的是下面两个：

```js
// 接收一个 CSS 选择符，返回与该模式匹配的第一个元素
querySelector('class-name');

// 接收一个 CSS 选择符，返回所有匹配的元素
querySelectorAll('class-name');
```

`querySelectorAll()`返回的是一个 NodeList 实例。

红宝书3版里提到的 `matchesSelector()` 是非标准的，进入标准的是`matches()`。这个方法匹配到结果就返回`true`，没匹配到就返回`false`。

### 11.2  元素遍历

可以通过 `firstElementChild`、`childElementCount`、`lastElementChild`等遍历元素。

### 11.3 HTML5

HTML5 相对于之前的 HTML 进行大量的改动，结合同样改动很大的 CSS 版本 CSS3，前端可以开发出比以前酷炫的多的多的现代页面。

与之相应的，HTML5 的大量改动推动了 JS 做出对应的扩展。

HTMLDocuments 类型有一个 `readyState`，这个属性在 HTML5 成为标准，该属性主要有两个值：

- loading，正在加载文档
- complete，已经加载完成文档

这个属性非常棒，可以实现一个文档加载指示器。

### 总结

DOM 扩展提供了两个重要方法遍历元素：

- querySelector()
- querySelectorAll()

