##  <script> 元素

要在HTML中使用JavaScript，必然要用到 `<script>` 标签，这个标签也是 Netscape 首先实现的。

关于 `<script>` 标签的属性，详细参考 [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script)，这里列几个

> **async：** 规定异步执行脚本（仅适用于外部脚本）
>
> **src：** 定义引用外部脚本的URI

`<script>` 标签比较有意思的一个点：

```javascript
<script>
    function fn (){
      alert('</script>')
    }
  </script>
```

因为按照解析嵌入式代码的规则，当浏览器遇到字符串 `"</script>"` 时，就会认为那是结束的 `</script>`标签，可以通过转义符可以解决。如下：

```javascript
 <script>
    function fn (){
      alert('<\/script>')
    }
  </script>
```

**注意：**

> 1、在 HTML 中，`<script>` 标签必须用 `</script>` 标签闭合
>
> 2、`<script>` 标签以 src 属性引入 JavaScript 时，标签内不能再有 js脚本，不会被执行

