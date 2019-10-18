## Node.js 快速入门

本章节一共33页，预计1个小时。

### 编写简单的 node 程序

编写一个 app.js，在里面写一些 JS 代码，然后打开 node.exe 或者一些gitbash之类的工具。

```shell
## 通过下面命令运行 app.js，即可看到控制台打印对应的信息
node app.js
```

 supervisor 可以帮助你实现这个功能，它会监视你对代码的改动，并自动重启 Node.js。 使用方法很简单，首先使用 npm 安装 supervisor： 

```shell
npm install -g supervisor 
```

然后可以使用 `supervisor` 启动应用程序：

```shell
supervisor app.js
```



###  异步式 I/O 与事件式编程 

 Node.js 最大的特点就是异步式 I/O（或者非阻塞 I/O）与事件紧密结合的编程模式 。

 多线程可以让 CPU 资源不被阻塞中的线程浪费 ， 而在非阻塞模式 下，线程不会被 I/O 阻塞，永远在利用 CPU。  多线程带来的好处仅仅是在多核 CPU 的情况下利用更多的核，而Node.js的单线程也能带来同样的好处。这就是为什么 Node.js 使用了单线程、非阻塞的事件编程模式。 

> Node.js 处理异步 I/O 时并不会阻断程序运行， 而只是将 I/O 请求发送给操作系统，继续执行下一条语句。当操作系统完成 I/O 操作时，以事件的形式通知执行 I/O 操作的线程，线程会在特定时候处理这个事件。为了处理异步 I/O，线程必须有事件循环，不断地检查有没有未处理的事件，依次给以处理。 

Node.js异步读取文件：

```js
//readfile.js
var fs = require('fs');
fs.readFile('file.txt', 'utf-8', function(err, data) {
 if (err) {
 console.error(err);
 } else {
 console.log(data);
 }
});
console.log('end.'); 

// 依次输出：end、文件内容
```

 fs.readFile 调用时所做的工作只是将异步式 I/O 请求发送给了操作系统，然后立即 返回并执行后面的语句，执行完以后进入事件循环监听事件。当 fs 接收到 I/O 请求完成的 事件时，事件循环会主动调用回调函数以完成后续工作。因此我们会先看到 end.，再看到 file.txt 文件的内容。 

Node.js也可以同步读取文件。

### 事件

 Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。在开发者看来，事件由 EventEmitter 对象提供。前面提到的 `fs.readFile` 和 `http.createServer` 的回调函数都是通过 EventEmitter 来实现的。

#### Node.js的事件循环机制

 Node.js 在什么时候会进入事件循环呢？答案是 Node.js 程序由事件循环开始，到事件循环结束，所有的逻辑都是事件的回调函数，所以 Node.js **始终在事件循环中**，程序入口就是 事件循环第一个事件的回调函数。 

 事件的回调函数在执行的过程中，可能会发出 I/O 请求或 直接发射（emit）事件，执行完毕后再返回事件循环，事件循环会检查事件队列中有没有未处理的事件，直到程序结束。 

Node.js 没有显示的事件循环。 Node.js 的事件循环对开发者不可见，由 libev 库实现。 

一系列的 libev 库在 Node.js 中由 EventEmitter 封装。

 libev 事件循环的每一次迭代，在 Node.js 中就是一次 Tick，libev 不 断检查是否有活动的、可供检测的事件监听器，直到检测不到时才退出事件循环，进程结束。 

事件循环如下图：

![1571392409164](C:\Users\luo19\AppData\Roaming\Typora\typora-user-images\1571392409164.png)



### 模块和包

 模块（Module）和包（Package）是 Node.js 最重要的支柱。开发一个具有一定规模的程 序不可能只用一个文件，通常需要把各个功能拆分、封装，然后组合起来，模块正是为了实 现这种方式而诞生的。 

 Node.js 提供了 require 函数来调用其他模块，而且模块都是基于 文件的，机制十分简单。 

模块和包没什么区别，唯一的区分的话，有时候包会用来指代某个功能模块的集合。

创建一个模块很简单：

```js
//module.js
function Hello() {
 var name;

 this.setName = function(thyName) {
 name = thyName;
 };

 this.sayHello = function() {
 console.log('Hello ' + name);
 };
};
module.exports = Hello; 
```

然后我们就可以在其它文件里使用模块了：

```js
//getmodule.js
var Hello = require('./hello');
hello = new Hello();
hello.setName('BYVoid');
hello.sayHello(); // Hello BYVoid
```

**注意：** 不要重复的`require`模块。require 会创建一个对象，多次引用会被最新的覆盖。

包在模块的基础上更进一步的封装、扩展，使得功能更强大，然后我们可以把包发布到 npm 上，让更多的人使用我们的包。

现在很多开发工具可以调试 node.js，比如 vscode。

 

### 总结

- Node.js 是异步式 I/O 与事件式编程
- Node.js 没有显示的事件循环，其机制被封装在了 EventEmitter 里。