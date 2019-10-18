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

