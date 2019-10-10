## TypeScript 教程

这是基于 [runoob](https://www.runoob.com/typescript/ts-tutorial.html) 版的快速学习笔记

### 简介

TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准。

TypeScript 由微软开发的自由和开源的编程语言。

TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上。

### TypeScript 安装

可以使用 npm 安装

```shell
## 全局安装
npm install -g typescript

## 查看版本
tsc -v
Version 3.2.2
```

通常我们使用 **.ts** 作为 TypeScript 代码文件的扩展名。

```typescript
// test.ts
var message:string = "Hello World" 
console.log(message)
```

然后通过下面命令将 TypeScript 转换为 JavaScript

```shell
tsc test.ts
```

转换成功后会在 test.ts 目录下生成同名的 test.js。

在大型项目开发中，我们当然应该利用 webpack 等工具结合相应的模块对 ts 文件批量处理转换。

### TypeScript 基础语法

TypeScript 程序由以下几个部分组成：

- 模块
- 函数
- 变量
- 语句和表达式
- 注释

ts 也有自己的关键字，基本跟 js 是一样的。

在 *空白/换行*、*分号*、*大小写*、*注释*、方面跟 js 是一样的。

关于类的写法，ts 采用了 ES6 的 class

```typescript
class Site { 
   name():void { 
      console.log("Runoob") 
   } 
} 
var obj = new Site(); 
obj.name();
```

### TypeScript 基础类型

js有的基础类型，ts 都有，ts多了几个：`any`、`enum`、`never`、`void`

ts 声明时带类型声明如下：

```typescript
// 关键字 变量名: 类型 = 赋值
let x: any = 1;
```

any 类型，任意值是 TypeScript 针对编程时类型不明确的变量使用的一种数据类型。任意值类型可以让这些变量跳过编译阶段的类型检查。

never 类型，never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环）。

```typescript
let x: never;
let y: number;

// 运行错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();
```

void 类型，用于标识方法返回值的类型，表示该方法没有返回值。

enum 类型，枚举类型用于定义数值集合。

```typescript
enum Color {Red, Green, Blue};
let c: Color = Color.Blue;
console.log(c);    // 输出 2
```

### TypeScript 变量声明

变量是一种使用方便的占位符，用于引用计算机内存地址。我们可以把变量看做存储数据的容器。

TypeScript 变量的命名规则（同 JS）：

- 变量名称可以包含数字和字母。
- 除了下划线 **_** 和美元 **$** 符号外，不能包含其他特殊字符，包括空格。
- 变量名不能以数字开头。

ts 变量使用前必须先声明，我们可以使用 var 来声明变量。（JS 引擎有声明提升，可以先使用变量后声明，但是原则上还是要在使用变量前声明变量，这样可以避免很多不必要的麻烦。）

声明变量的4种方式：

```typescript
// 声明变量的类型及初始值：
var [变量名] : [类型] = 值;

// 声明变量的类型及但没有初始值，变量值会设置为 undefined
var [变量名] : [类型];

// 声明变量并初始值，但不设置类型类型，该变量可以是任意类型
var [变量名] = 值;

// 声明变量没有设置类型和初始值，类型可以是任意类型，默认初始值为 undefined
var [变量名];
```

这种声明变量的方式跟 js 是一样的，只是比 js 多了个类型。

TypeScript 遵循强类型，如果将不同的类型赋值给变量会编译错误，如下实例：

```typescript
var num:number = "hello"     // 这个代码会编译错误
```

### 类型断言

类型断言可以用来手动指定一个值的类型，即允许变量从一种类型更改为另一种类型。

语法格式用 `<类型>值` 或 `值 as 类型`

```typescript
var str = '1' 
var str2:number = <number> <any> str   //str、str2 是 string 类型
console.log(str2)
```

类型断言用的不好容易出问题，所以在不确定变量类型的情况下，最好用 `any`，稳的很。

> 它之所以不被称为**类型转换**，是因为转换通常意味着某种运行时的支持。但是，类型断言纯粹是一个编译时语法，同时，它也是一种为编译器提供关于如何分析代码的方法。

### 类型推断

当类型没有给出时，TypeScript 编译器利用类型推断来推断类型。

如果由于缺乏声明而不能推断出类型，那么它的类型被视作默认的动态 any 类型。

```typescript
var num = 2;    // 类型推断为 number
console.log("num 变量的值为 " + num); 
num = "12";    // 编译错误
console.log(num);
```

### 变量作用域

变量作用域指定了变量定义的位置。

程序中变量的可用性由变量作用域决定。

TypeScript 有以下几种作用域：

- **全局作用域** − 全局变量定义在程序结构的外部，它可以在你代码的任何位置使用。
- **类作用域** − 这个变量也可以称为 **字段**。类变量声明在一个类里头，但在类的方法外面。 该变量可以通过类的对象来访问。类变量也可以是静态的，静态的变量可以通过类名直接访问。
- **局部作用域** − 局部变量，局部变量只能在声明它的一个代码块（如：方法）中使用。

作用域方面的限制， ts 比 js 严格些。