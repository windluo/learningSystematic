一个模块就是一个独立的文件，内部的属性、方法对外都是不可见的，只有通过`export`导出才可见，外部通过`import`导入使用。

```js
// 第一组
export default function crc32() { // 输出
  // ...
}

import crc32 from 'crc32'; // 输入

// 第二组
export function crc32() { // 输出
  // ...
};

import {crc32} from 'crc32'; // 输入
```

上面代码的两组写法，第一组是使用`export default`时，对应的`import`语句不需要使用大括号；第二组是不使用`export default`时，对应的`import`语句需要使用大括号。

`export default`命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此`export default`命令只能使用一次。所以，import命令后面才不用加大括号，因为只可能唯一对应`export default`命令。

本质上，`export default`就是输出一个叫做`default`的变量或方法，然后系统允许你为它取任意名字。

**注意：**`export/import *`会导出/导入模块的所有属性和方法，但是会忽略模块的`default`方法。

#### 复合写法

如果在一个模块之中，先输入后输出同一个模块，`import`语句可以与`export`语句写在一起。

```javascript
export { foo, bar } from 'my_module';

// 可以简单理解为
import { foo, bar } from 'my_module';
export { foo, bar };
```

#### CommonJS 和 ES6 模块的差异

主要是两点：

- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

#### 循环加载的问题

模块之间相互引用无法避免，不同模块机制有其不同的处理方式。ES6的处理方式是声明提升。

以下面代码为例：

```js
// a.js
import {bar} from './b';
console.log('a.mjs');
console.log(bar());
function foo() { return 'foo' }
export {foo};

// b.js
import {foo} from './a';
console.log('b.mjs');
console.log(foo());
function bar() { return 'bar' }
export {bar};
```

`a.js`引用`b.js`，`b.js`又引用了`a.js`，ES6模块的处理办法就是先执行`a.js`，发现引用了`b.js`，优先执行`b.js`，在`b.js`里发现引用了`a.js`的函数`foo()`，因为函数声明有提升作用，在执行`import {bar} from './b'`时，函数`foo`就已经有定义了，所以`b.js`加载的时候不会报错。

> 即在解析代码时，提升变量的声明。
>
> **注意：**函数表达式不具有提升作用，会报错。所以模块里的函数一定要写成函数声明形式的。
>
> > 模块里的函数表达式报错问题，我之前遇到过，后来改成函数声明即可。

CommonJS 模块的重要特性是加载时执行，即脚本代码在`require`的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。