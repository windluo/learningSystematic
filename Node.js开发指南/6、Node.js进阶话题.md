## Node.js进阶话题

本章节一共16页，预计30分钟。

### 模块加载机制

加载模块直接使用 `require`即可。

Node.js 的模块分为两大类：核心模块和文件模块。

核心模块是 Node.js 标准 API 中提供的模块，如`fs`、`http`等。 我们可以直接通过 require 获取核心模块，例如 require('fs')。核心模块拥有最高的加载优先级，换言之如果有模块与其命名冲突， Node.js 总是会加载核心模块。 

文件模块则是存储为单独的文件（或文件夹）的模块，可能是 JavaScript 代码、JSON 或 编译好的 C/C++ 代码。

> JavaScript 文件，*.js
>
> Json 文件，*.json
>
> C/C++ 文件，*.node

`require`加载时，遇到`require('fs')`这种，会优先去` node_modules `文件夹查找。在找不到的情况下，会依次往上查找到根目录。

 Node.js 模块不会被重复加载， 是因为 Node.js 通过文件名缓存所有加载过的文件模块。

>  即使你分别通过 require('express') 和 require('./node_modules/express') 加载两次，也不会重复加载，因为尽管两次参数不同，解析到的文件却是同一个。 

### 控制流

 基于异步 I/O 的事件式编程容易将程序的逻辑拆得七零八落，给控制流的疏理制造障 碍。 

#### 循环的陷阱

这个是异步机制导致的问题，主要体现在 `for...`循环：

```js
var fs = require('fs');
var files = ['a.txt', 'b.txt', 'c.txt'];
for (var i = 0; i < files.length; i++) {
 	fs.readFile(files[i], 'utf-8', function(err, contents) {
 	console.log(files[i] + ': ' + contents);
 })
```

因为异步的原因，导致最后回调函数里的`files[i]`里的`i`其实取得是最后一次`i++`，所有的`files[i]`都会打印`undefined`，文件内容可以获取到。

这里可以采用匿名函数处理，最佳方案推荐使用`forEach`，利用它的匿名函数结合循环：

```js
var fs = require('fs');
var files = ['a.txt', 'b.txt', 'c.txt'];
files.forEach(function(filename) {
 	fs.readFile(filename, 'utf-8', function(err, contents) {
 	console.log(filename + ': ' + contents);
  });
}); 
```

#### 解决控制流难题

 Node.js 异步式编程还有一个显著的问题，即深层的回调函数嵌套。 在这种情况下，我们很难像看基本控制流结构一样一眼看清回调函数之间的关系，因此当程序规模扩大时必须采取手段降低耦合度，以实现更加优美、可读的代码。 

一般的解决方案会引入一些模块：` async `、` streamlinejs `等。

### Node.js 应用部署

本书上提到一些不足之处现在都有了对应的解决方案。最常用的是利用`pm2`部署Node.js，它可以自动重启、还可以开多线程。

### Node.js 不适合做什么

- 计算密集型的程序 
- 单用户多任务型应用 
- 逻辑十分复杂的事务 
- Unicode 与国际化 

