## Node.js核心模块

本章节一共20页，预计1个小时。

### 全局对象

Node.js 的全局对象是 `global`。JS 的全局对象是 `window`。

`process`是一个全局变量， 用于描述当前 Node.js 进程状态 的对象，提供了一个与操作系统的简单接口。 

 `process.stdout`和` process.stdin `是标准输出/输入流，它们能够让我们进行更底层的输入/输出。

 `process.nextTick(callback)`的功能是为事件循环设置一项任务，Node.js 会在下次事件循环响应时调用 callback。 

>  Node.js 是异步式 I/O 编程，`process.nextTick(callback)`可以让我们把复杂的事件拆散，变成一个个较小的事件。

 `process`还有其它方法，如：process.platform、 process.pid、process.execPath、process.memoryUsage() 等。

### 常用工具 util

 util 是一个 Node.js 核心模块，提供常用函数的集合，用于弥补核心 JavaScript 的功能 过于精简的不足。 

#### util.inherits

 `util.inherits(constructor, superConstructor)`是一个实现对象间原型继承的函数。 

```js
var util = require('util');
function Base() {
  this.name = 'base';
  this.base = 1991;

  this.sayHello = function() {
    console.log('Hello ' + this.name);
  };
}
Base.prototype.showName = function() {
  console.log(this.name); 
};
function Sub() {
  this.name = 'sub';
}

util.inherits(Sub, Base);
var objBase = new Base();
objBase.showName();
objBase.sayHello();
console.log(objBase);
var objSub = new Sub();
objSub.showName();
//objSub.sayHello();  // 构造函数内的继承不到。
console.log(objSub); 
```

使用`util.inherits`只能继承父对象定义在原型`prototype`上的属性和方法，构造函数内的不会继承。

> 如果用 ES6 的 `class`定义对象，可以通过`extend`实现继承。

`util`还提供了一系列的其它方法，可以在 Node.js 官网查看。

### 事件驱动 events

 events 是 Node.js 最重要的模块，没有“之一”，原因是 Node.js 本身架构就是事件式 的，而它提供了唯一的接口，所以堪称 Node.js 事件编程的基石。events 模块不仅用于用户代码与 Node.js 下层事件循环的交互，还几乎被所有的模块依赖。 

 events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件发射与事件监听器功能的封装。

 对于每个事件，EventEmitter 支持 若干个事件监听器。当事件发射时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。 

```js
var events = require('events');
var emitter = new events.EventEmitter();
emitter.on('someEvent', function(arg1, arg2) {
  console.log('listener1', arg1, arg2);
});
emitter.on('someEvent', function(arg1, arg2) {
  console.log('listener2', arg1, arg2);
});
emitter.emit('someEvent', 'byvoid', 1991); 
```

打印结果如下：

```js
listener1 byvoid 1991
listener2 byvoid 1991 
```

#### error 事件

 EventEmitter 定义了一个特殊的事件 error，它包含了“错误”的语义，我们在遇到异常的时候通常会发射 error 事件。 

`fs`、`net`、`http`等模块都是继承自`events. EventEmitter `。



### 总结

Node.js 的事件循环依赖的就是 `events `模块，这个模块是它最核心的功能。

events 模块不仅用于用户代码与 Node.js 下层事件循环的交互，还几乎被所有的模块依赖。 