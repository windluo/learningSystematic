## 第21章、Ajax 与 Comet

本章内容

> 使用 XMLHttpRequest 对象
>
> 使用 XMLHttpRequest 事件
>
> 跨域 Ajax 通信的限制

Ajax 是一种无须刷新页面即可从服务器端获取数据的技术。其核心是 XMLHttpRequest 对象。

### 21.1 XMLHttpRequest 对象

XMLHttpRequest 对象简称 XHR。使用方法很简单：

```js
var xhr = new XMLHttpRequest();

// 启动请求
xhr.open("get", "example.php", false); 
// 发送请求
xhr.send(null); 
```

可以通过检测 XHR 对象的 readyState 属性判断请求到了哪个阶段。readyState 有以下状态值：

- 0，未初始化。尚未调用 open() 方法。
- 1，启动。已经调用 open() 方法，尚未调用 send() 方法。
- 2，发送。已经调用 send() 方法，尚未接收到响应。
- 3，接收。已经接收到部分响应数据。
- 4，完成。已经接收到全部响应数据，且已经可以在客户端使用了。

readyState 属性的值由一个值变成另一个值就会触发一次 `readystatechange`事件。可以利用这个事件检测状态变化。

在接收到响应之前还可以调用`abort()`方法取消异步请求。

在发送请求前，可以通过`setRequestHeader()`设置请求头信息，能够设置的信息包括`Cookie`、`Accept-Encoding`等等。

请求方式有`get`、`post`、`delete`等等。

### 21.2 XMLHttpRequest 2级

XHR 1级只是把已有的 XHR 对象的实现细节描述了出来，XHR 2级则进一步发展了 XHR。

#### FormData

增强之一就是引入了`FormData`类型增强表单数据的序列化。

```js
// 自定义数据
var data = new FormData();
data.append("name", "Nicholas"); 

// 或者序列化已存在的 form 表单
var data = new FormData(document.forms[0]); 

// 发送
xhr.send(data); 
```

#### 超时设定

`timeout` 超时属性表示请求在等待多少毫秒后就终止。可以通过`ontimeout`事件监听处理程序。

#### overrideMimeType() 方法

overrideMimeType() 方法用于重写 XHR 响应的 MIME 类型。比如服务器返回的类型是 `text/plain`，但数据中实际包含的是 XML，可以通过`overrideMimeType`事件重写该类型，保证把响应当作 XML 处理。

```js
var xhr = createXHR();
xhr.open("get", "text.php", true);
xhr.overrideMimeType("text/xml");
xhr.send(null); 
```

### 21.3 进度事件

Progress 事件定义了与客户端服务器通信有关的事件。有以下6个进度事件。

- loadstart，在接收到响应数据的第一个字节时触发。
- progress，在接收响应期间持续不断地触发。可以通过这个处理进度百分比。
- error，请求发生错误时触发。
- abort，调用`abort()`方法而终止连接时触发。
- load，在接收到完整的响应数据时触发。
- loadend，通信完成或触发 error、abort 或 load 事件后触发。

### 21.4 跨域资源共享

默认情况下，XHR 对象只能访问与包含它的页面位于同一个域中的资源。这种安全策略可以防止某些恶意行为。

跨域资源共享（CORS）定义了在必须访问跨域资源时，浏览器与服务器该如何沟通。通过在 Http 请求头加上`Origin`头信息，同时服务器要设置`Access-Control-Allow-Origin`，服务器和客户端的 Origin 头匹配即可跨域请求资源。

一般需要设置如下信息：

```js
Origin: http://www.nczonline.net  // 请求源
Access-Control-Request-Method: POST // 请求方式
Access-Control-Request-Headers: NCZ // 请求头
```

### 21.5 其他跨域技术

#### 图像Ping

一个网页可以从任何网页中加载图像，不用担心跨域问题。图像Ping是与服务器进行简单、单向的跨域通信的一种方式。可以通过动态创建图像实现图像Ping。

```js
var img = new Image();
img.onload = img.onerror = function(){
 alert("Done!");
};
img.src = "http://www.example.com/test?name=Nicholas"; 
```

图像Ping最常用于跟踪用户点击页面或动态广告次数。但是它只能接收 Get 请求，且无法访问服务器的响应文本，因此只能用于简单的单向通信。

#### JSONP

JSONP 是 JSON with padding（填充式 JSON 或参数式 JSON）的简写。它是包含在函数调用中的 JSON，如下：

```js
function handleResponse(response){
 alert("You're at IP address " + response.ip + ", which is in " +
 response.city + ", " + response.region_name);
}
var script = document.createElement("script");
script.src = "http://freegeoip.net/json/?callback=handleResponse";
document.body.insertBefore(script, document.body.firstChild); 
```

与图形Ping相比，它的优点在于能够直接访问响应文本，支持浏览器与服务之间的双向通信。

缺点是它从其他域加载代码，如果其他域不安全，夹杂恶意代码，则只能放弃 JSONP。

### Comet

Ajax 是一种从页面向服务器请求数据的技术，Comet 是一种服务器向页面推送数据的技术。

实现 Comet 的方式有两种：长轮询和流。

管理 Comet 的连接很容易出错，社区对其又进行了更新。

#### 服务器发送事件

服务器发送事件（SSE）利用的是 `EventSource`对象：

```js
var source = new EventSource("myevents.php"); 
source.onmessage = function(event){
 	var data = event.data;
    // 处理数据
}; 
```

#### Web Sockets

最令人津津乐道的新 API，Web Sockets在一个单独的持久连接上提供全双工、双向通信。

#### SSE还是Web Sockets

SSE对服务器要求不高，只需要服务器支持HTTP通信即可，Web Sockets则需要服务器的支持。

浏览器兼容性上，Web Sockets 比 SSE的`EventSource`要好的多。

> IE 和 Edge不支持`EventSource`。

### 安全

Ajax 请求的安全性一般会通过 cookie、token这些验证机制控制。



### 总结

- Ajax 是一种无须页面刷新即可从服务器获取数据的技术
- Comet 是一种从服务器向页面推送数据的技术
- Web Sockets 很棒

